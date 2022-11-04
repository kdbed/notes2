+++
title = "sha1Cracker"
author = ["svejk"]
draft = false
+++

## Implementing a [sha1]({{<relref "sha1.md#" >}}) hash cracker in Rust {#implementing-a-sha1--sha1-dot-md--hash-cracker-in-rust}

```rust

use sha1::Digest;
use std::{
    env,
    error::Error,
    fs::File,
    io::{BufRead, BufReader},
};

const SHA1_HEX_STRING_LENGTH: usize = 40;

fn main() -> Result<(), Box<dyn Error>> {

    let args: Vec<String> = env::args().collect();

    if args.len() != 3 {
        println!("Usage:");
        println!("sha1_cracker: <wordlist.txt> <sha1_hash>");
        return Ok(());
    }

    let hash_to_crack = args[2].trim();
    if hash_to_crack.len() != SHA1_HEX_STRING_LENGTH {
        return Err("sha1 hash is not valid.".into());
    }

    let wordlist_file = File::open(&args[1])?;
    let reader = BufReader::new(&wordlist_file);

    for line in reader.lines() {
        let line = line?;
        let common_password = line.trim();
        if hash_to_crack == &hex::encode(sha1::Sha1::digest(common_password.as_bytes())) {
            println!("Password found: {}", &common_password);
            return Ok(());
        }
    }

    println!("password not found in wordlist.");

    Ok(())

}

```


### Imports {#imports}

<a id="code-snippet--imports"></a>
```rust
use sha1::Digest;
use std::{
    env,
    error::Error,
    fs::File,
    io::{BufRead, BufReader},
};

const SHA1_HEX_STRING_LENGTH: usize = 40;
```


### Inputs {#inputs}

<a id="code-snippet--inputs"></a>
```rust
let args: Vec<String> = env::args().collect();

if args.len() != 3 {
    println!("Usage:");
    println!("sha1_cracker: <wordlist.txt> <sha1_hash>");
    return Ok(());
}

let hash_to_crack = args[2].trim();
if hash_to_crack.len() != SHA1_HEX_STRING_LENGTH {
    return Err("sha1 hash is not valid.".into());
}
```


### Read File {#read-file}

<a id="code-snippet--readFile"></a>
```rust
let wordlist_file = File::open(&args[1])?;
let reader = BufReader::new(&wordlist_file);
```


### Check Pass {#check-pass}

<a id="code-snippet--checkPass"></a>
```rust
for line in reader.lines() {
    let line = line?;
    let common_password = line.trim();
    if hash_to_crack == &hex::encode(sha1::Sha1::digest(common_password.as_bytes())) {
        println!("Password found: {}", &common_password);
        return Ok(());
    }
}
```


## Bibliography {#bibliography}

Black Hat Rust
