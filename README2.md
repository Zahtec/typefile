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
The proper [MIME](https://en.wikipedia.org/wiki/Media_type) type of TypeFile is `text/typefile`.

## Docs

**Chapters:**  
- [Comments](https://github.com/Zahtec/typefile/blob/main/README2.md#comments)
- [Key/Value Pairs](https://github.com/Zahtec/typefile/blob/main/README2.md#keyvalue-pairs)
    - Keys
    - Values
- Objects
    - The root object
    - Subobjects
    - Inline objects
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
```
Since whitespace is ignore, the TFI sees the above data as:
```
# Keep in mind "value" is not a real value, and "type" is not a real type. They are only used as an example.
key=value@type
```
So you can style it any way you want. My preffered styling is:
```
# Keep in mind "value" is not a real value, and "type" is not a real type. They are only used as an example.
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
Blank/Unspecific values/keys are dissalowed. This includes empty quoted keys.
```
# Keep in mind "value" is not a real value. It is only used as an example.
# These all throw errors.
key =
 = value
 "" = value
 ```
### Keys
A key may either be bare or quoted.  
Bare keys may only contain ASCII letters, ASCII digits, underscores, and dashes (`A-Za-z0-9_-`). And should always be typed in [Camel Case](https://en.wikipedia.org/wiki/Camel_case). Note that bare keys are allowed to be composed of only ASCII digits, but are always interpreted as strings by the TFI.
```
# These are all valid.
key = "value"
bare_key = "value"
bare-key2 = "value"
1234 = "value"
```
Quoted keys allow you to use a much broader amount of characters. Including special characters and whitespace, although using whitespace is discouraged.
```
# These are all valid.
"white space" = "value" # Okay but discouraged, use "white_space" or "white-space" instead.
"ʎǝʞ" = "value"
"key" = "value"
"100$" = "broke"
```
Defining a key multiple times is dissalowed. Bare keys and quoted keys are equivalent for this matter.
```
# These all throw an error.
key = "value"
key = "value2"
# And so does this.
"key" = "value3"
```
### Values
Values can be any one of these types:
- String
- Integer
- Float
- Boolean
- Offset Date-Time
- Local Date-Time
- Local Date
- Array

For strings, you can use single quotes (`'`) or double quotes (`"`). When using single quotes, you can nest double quotes inside of that string, and vice-versa. Note that 2 of the same type of quotes must be at the start and end of a string.
```
# These are valid.
key = 'value'
key2 = "value"
nested = '"value" -Zahtec'
# This throws an error.
key3 = 'value"
```
To declare a type for a value, use an `@` symbol. Everything after the `@` will be treated as a type (excluding comments).
```
# These are all valid.
key = "value" @ string
key2 = 1 @ integer
key3 = false @ boolean
```
### Objects
Objects are collections of key/value pairs known as properties. They are defined by headers, with square brackets on either side of a name like so: `[object]`. Object headers must always have a name and follow their names follow the same syntax as keys. Objects must always be on a line by themselves (excluding comments and index signatures). Objects should always be typed in [Camel Case](https://en.wikipedia.org/wiki/Camel_case).
```
# Example object header.
[object]
```
Under the object header, and until the EOF (End of file) or next object header, are either the properties (key/value pairs) of that object or its subobjects with their own respective properties.
```
# These are all valid.
[object]
prop = "value"
:[subobject]
prop = "value"

["object titles are like keys"]
key = "value"
```
#### The Root Object
The root object is the main object the file is represented as. All root object properties must be before the first non-root object or EOF.
```
# This is in the root object.
prop = "value"

[object]
# This is not.
prop = "value"
```
As you may have noticed, there are duplicate properties (keys) in our file, which is dissalowed. Or so it may seem. Every object has its own respective properties, and those can not be named the same.
```
# These throw an error.
prop = "value"
prop = "value2"
```
But the same property across multiple objects is permitted.
```
# These are all valid.
# These properties are in the root object.
prop = "value"
prop2 = "value"

[object]
# While these are in the "object" object.
prop = "value"
```
Whitespace around the object name is ignored. However, best practice is to not use any extra, uneeded whitespace.
```
# These are all valid.
[a b c]
[ d e f ]
[ g        h            i ]
# If needed, for spacing, use a quoted object title.
["I like space"]
```
Like keys, empty objects and object names are dissalowed and duplicate objects are dissalowed aswell. Empty objects count as objects with no properties or subobjects until the EOF or next object header.
```
# These all throw an error.
[""]
prop = "value"
[]
prop = "value"
[empty]

[object]
prop = "value"
[object]
prop = "value2"
```
#### Subobjects
Subobjects are objects within objects. This can go on infinitely. Subobjects are defined by putting an `:` before an object header and follow all the same concepts that a regular object does. The count of how many `:` characters to use is based on how deep that subobject is. Note that a subobject must be inside an object, the root object does not support them.
```
# These are all valid.
[object]
prop = "value"
:[subobject]
prop = "value"
::[subsubobject]
prop = "value"
```
For readability, since whitespace is ignored, you may style it however you like. Like so:
```
# These are all valid.
[object]
prop = "value"
    :[subobject]
        prop = "value"
        ::[subsubobject]
            prop = "value"
```
Here is the same data but in [JSON](https://www.json.org):
```
{
    "object": {
        "prop": "value",
        "subobject": {
            "prop": "value",
            "subsubobject": {
                "prop": "value"
            }
        }
    }
}
```
The root object does not support subobjects.
```
# This throws an error.
:[subobject]
prop = "value"
```
The amount of `:` characters before your subject depends on how deep it is.
```
# These are all valid.
[object]
prop = "value"
:[subobject-depth-1]
prop = "value"
::[subobject-depth-2]
prop = "value"
:[subobject2-depth-1]
prop = "value"
```
Here is the same data but in [JSON](https://www.json.org):
```
{
	"object": {
		"prop": "value",
		"subobject-depth-1": {
			"prop": "value",
			"subobject-depth-2": {
				"prop": "value"
			}
		},
		"subobject2-depth-1": {
			"prop": "value"
		}
	}
}
```
#### Inline objects
Inline objects provide a more compact syntax for expressing tables. Inline objects are defined by inline curly braces `{` and `}`. Within the braces, the same rules apply as regular objects, and one or more comma-separated key/value pairs (properties) may appear. Inline objects must always be inline and can not be spread across multiple lines.
```
# Example inline object.
object = { prop = "value", prop2 = "value2" }
```
This is the same as:
```
# These are all valid.
[object]
prop = "value"
prop2 = "value2"
```
Inline objects can also be used with subobjects. 
```
# These are all valid.
inline = { prop = "value", subobject = { prop = "value" } }
```
This is the same as:
```
# These are all valid.
[inline]
prop = "value"
:[subobject]
prop = "value"
```
Inline objects are self-contained and define all properties and subobjects within them.
```
# This is valid.
inline = { prop = "value", subobject = { prop = "value" } }
# This will throw an error.
[inline]
prop = "value"
```
When in the root object, inline objects cannot be used to add properties or subobjects to an already defined object.
```
# This is valid.
object = { prop = "value" }
# This will throw an error.
[object]
prop = "value"
# This is valid.
[inline]
inline = { prop = "value" }
# This will create inline.inline.prop which is equal to "value". It is highly discouraged.
```
.. continue ..
### Types
- Type Interpretation
- Index Signatures
- String
- Integer
- Float
- Boolean
- Offset Date-Time
- Local Date-Time
- Local Date
- Array
