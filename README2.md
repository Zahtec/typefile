# TypeFile
**!! THIS IS A CONCEPT !!**  
A strongly typed configuration/data storage file built for languages like [TypeScript](https://www.typescriptlang.org) and [Java](https://www.java.com).

## What is TypeFile?
TypeFile aims to be a human readable, strongly typed, super compatible data storage/configuration file for everyone. Built on OSS, typefile is aimed to combine [TOML](https://github.com/toml-lang/toml), [JSON](https://www.json.org), and [YAML](https://yaml.org), into one simple file type.

## How do I use it?
To get started with TypeFile, you need the Type File Interpreter or TFI for your preffered language. This will read/write data to and from your TypeFile (`.tf`) in the correct manner.

For [Node.js](https://nodejs.org): Get the [npm](https://www.npmjs.com) package here (coming soon).  
For [Deno](https://deno.land): Get the [npm](https://www.npmjs.com) package here (coming soon).  
For [Python](https://www.python.org): Get the [pip](https://pip.pypa.io) package here (coming soon).  
For [Rust](https://www.rust-lang.org): Get the [cargo](https://crates.io) crate here (coming soon).  
**These are the official TFI's for all supported languages**

[IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) extensions:  
[VSC](https://code.visualstudio.com): Install the extension here (coming soon).  
[IDEA](https://www.jetbrains.com/idea): Install the extension here (coming soon).  
**These only enforce syntax and can not be used instead of a TFI**

## Requirements
TypeFile must be in a UTF-8 encoded file.  
TypeFiles must always end in `.tf`.

## Spec
TypeFile is case-sensitive.  
TypeFile will ignore all whitespace (Tabs and spaces) except for inside a string.  
NewLines (LF or CRLF) do not count as whitespace and are required.

## Docs

**Chapters:**  
- [Comments](https://github.com/Zahtec/typefile/blob/main/README2.md#comments)
- [Key/Value Pairs](https://github.com/Zahtec/typefile/blob/main/README2.md#comments)
- Objects
- Types

### Comments

Comments are defined by using the `#` character. After the `#` the rest of the line will be a comment.
```
# Hi im a comment
key = 'value' # Look at the key/value pair to the left of me!
```
To make a multi-line comment, use three `#`'s. After three `#`'s, the rest of the file will be a comment until a repeating three `#`'s appears. Not putting the last three `#`'s before the EOF (End of file) will result in an error.
```
### I span
    Over Multiple
    Lines ###

### I cause
    an error
```
### Key/Value pairs
Keys are on the left side of the equals sign, while values are on the right. Type declaration is always after the values on the right and is initialized using the `@` symbol. The key, equals sign, value, and type declaration must be on the same line. Also, types must not be incorrect to the value or else the it will throw an error.
```
# Keep in mind "value" is not a real value, and "type" is not a real type. They are only used as an example.
key = value@type
# Since whitespace is ignore, the TFI sees this as:
key=value@type
# So you can style it any way you want. My preffered styling is:
key = value @ type
```
Here is a small example with real values and types:
```
name = "zahtec" @ string
integer = 1 @ integer
```
Here is the same data but in [JSON](https://www.json.org):
```
{
    "name": "zahtec",
    "integer": 1
}
```
