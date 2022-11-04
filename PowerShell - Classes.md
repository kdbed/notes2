

## [[PowerShell]] Classes


### Introduction

PowerShell is an object-oriented language. When you run commands, see the output on your screen, those are objects.

Skeleton of a class called `student`:

```powershell
class student {

        }
```

Classes have properties that look like parameters that are attributes that describe that class. The example below shows a class called student with two properties; `FirstName` and `LastName`.

When you define a property, you should always define a type that sets a specific _schema_ for what property values can hold. In the example below, both properties are defined as strings.


### Student Class 

Class with two properties:

```powershell
class student {
    [string]$FirstName
    [string]$LastName
}
```

After you define a class, create an object from it or instantiate an object. There are multiple ways to instantiate objects from classes; one common way is to use type accelerators such as _student_  which represent the class, followed by a default method that comes with every class called new().

Using the type accelerator shortcut is the same as creating an object using the New-Object command. ==

Once you've created an object from that class, then assign values to properties. The example below is assigning values of _Tyler_ and _Muir_ for the `FirstName` and `LastName` properties.

```powershell
class student {
    [string]$FirstName
    [string]$LastName
}
$student1 = [student]::new()
$student1.FirstName = 'Tyler'
$student1.LastName = 'Muir'
$student1 | Get-Member
$student1 | Write-Output
```

Notice that Get-Member returns four methods and two properties. The properties probably look familiar, but the methods sure don't. PowerShell adds certain methods by default, but you can add your own methods or even modify the default methods.


### Methods {#methods}

Defining methods with a _scriptblock_

```powershell
[<output type>]<name>() {
	<code that runs when the method is executed>
}
```

Method parameters set set inside the parentheses ().


#### `return` is mandatory {#return-is-mandatory}

PowerShell functions will return objects by simply placing the object anywhere in the function; if a method returns an object, you must use the return construct as shown below.

```powershell
[string]GetName() {
	return 'foo'
}
```


## References {#references}

Taken from: [ATA](https://adamtheautomator.com/powershell-classes/)