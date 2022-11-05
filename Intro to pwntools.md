---
title: Intro to pwntools
tags:
  - binaryAnalysis
  - infosec
  - pentest
  - tryHackMe
---




Basics of [[binary_analysis]]

## Intro to Pwntools

Source materials here [dizmascyberlabs
](https://github.com/dizmascyberlabs/IntroToPwntools)
[Install gdb-pwndbg-peda-gef](https://github.com/apogiatzis/gdb-peda-pwndbg-gef)


### Checksec

Same source code, compiled with different protections in place:

```bash
checksec checksec/intro2pwn2
```


```text
[*] '/home/kdb/Downloads/IntroToPwntools/IntroToPwntools/checksec/intro2pwn2'
    Arch:     i386-32-little
    RELRO:    Partial RELRO
    Stack:    No canary found
    NX:       NX disabled
    PIE:      No PIE (0x8048000)
    RWX:      Has RWX segments
```


```
checksec checksec/intro2pwn1
```


```text
[*] '/home/kdb/Downloads/IntroToPwntools/IntroToPwntools/checksec/intro2pwn1'
    Arch:     i386-32-little
    RELRO:    Full RELRO
    Stack:    Canary found
    NX:       NX enabled
    PIE:      PIE enabled
```

-   [RELRO]({{<relref "relro.md#" >}})  = Relocation Read-Only; makes the global offset table (GOT) read-only after the linker resolves functions to it. The GOT is important for techniques such as the ret-to-libc attack
-   [Stack Canaries]({{<relref "stack_canaries.md#" >}}) = tokens placed after a stack to detect a stack overflow. Stack canaries sit beside the stack in memory (where the program variables are stored), and if there is a stack overflow, then the canary will be corrupted. This allows the program to detect a buffer overflow and shut down.
-   [NX]({{<relref "nx.md#" >}}) = NX is short for non-executable. If this is enabled, then memory segments can be either writable or executable, but not both. This stops potential attackers from injecting their own malicious code (called shellcode) into the program, because something in a writable segment cannot be executed.  On the vulnerable binary, you may have noticed the extra line RWX that indicates that there are segments which can be read, written, and executed. [wiki](https://en.wikipedia.org/wiki/Executable%5Fspace%5Fprotection)
-   [PIE]({{<relref "pie.md#" >}}) = PIE stands for Position Independent Executable. This loads the program dependencies into random locations, so attacks that rely on memory layout are more difficult to conduct.  [redhat](https://access.redhat.com/blogs/766093/posts/1975793)

Resource for properties involved in `checksec` : [siphos](https://blog.siphos.be/2011/07/high-level-explanation-on-some-binary-executable-security/)


### Cyclic

If we look at the files and permissions in the cyclic directory:

![Image](cyclic1.jpg)

You will see that the flag file and intro2pwn3 are owned by the same user, and that the suid bit is set for intro2pwn3. This means that the program will keep its permissions when it executes.

Taking a look at the c code:

```
cat cyclic/test_cyclic.c
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

void print_flag() {
	printf("Getting Flag:\n");
	fflush(stdout);
	char *cat_flag[3] = {"/bin/cat", "flag.txt", NULL};
	execve("/bin/cat", cat_flag,  NULL);
	exit(0);
}

void start(){
	char name[24];
	gets(name);
}


int main(){
	printf("I run as dizmas.\n");
	printf("Who are you?: ");
	start();

}
```

We see that although we (buzz) can run this program, the `print_flag()` function isn't executed. This program is vulnerable to a buffer overflow, because it uses the gets() function, which does not check to see if the user input is actually in bounds. In our case, the name variable has 24 bytes allocated, so if we input more than 24 bytes, we can write to other parts of memory. An important part of the memory we can overwrite is the instruction pointer (IP), which is called the eip on 32-bit machines, and rip on 64-bit machines. The IP points to the next instruction to be executed, so if we redirect the eip in our binary to the `print_flag()` function, we can print the flag.


#### Cyclic 

To control the IP, the first thing we need do is to is overflow the stack with a pattern, so we can see where the IP is.  A file `alphabet` is provided; start gdb with `gdb intro2pwn3` and run with the alphabet file `r < alphabet`:

![Image](over1.jpg)

We've caused a segmentation fault, and you may observe that there is an invalid address at _0x4a4a4a4a_. If you scroll up, you can see the values at each register. For eip, it has been overwritten with _0x4a4a4a4a_.

The alphabet file can be produced with `cyclic 100`.

```
cyclic 100
```



```text
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaa
```