# TypeFile

A strongly typed data storage file built for the modern age. With an easy to read syntax, fast parser for supported languages, and docs you can actually read.

## ℹ What is TypeFile?

TypeFile aims to be a human readable, strongly typed, super compatible data storage file for everyone. It is a combination of [TOML](https://github.com/toml-lang/toml), [JSON](https://www.json.org), and [YAML](https://yaml.org), into one simple file type (Most of the combinations include styling/file structure).

## 📦 How do I use it?

To get started with TypeFile, you need the TypeFile Parser or TFP for your preferred language. This will read/write data to and from your TypeFile (`.tf`) in the correct manner. Anyone who is willing to create a parser for TypeFile in their preferred language is welcome to do so.

TypeFile Parsers:  
For [Node.js](https://nodejs.org): Get the [npm](https://www.npmjs.com) package here (coming soon).  
For [Deno](https://deno.land): Get the [deno](https://deno.land/x) package here (coming soon).  
For [Python](https://www.python.org): Get the [pip](https://pip.pypa.io) package here (coming soon).  
For [Rust](https://www.rust-lang.org): Get the [cargo](https://crates.io) crate here (coming soon).  

[IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) extensions:  
For [VSC](https://code.visualstudio.com): Install the extension here (coming soon).  
For [IDEA](https://www.jetbrains.com/idea): Install the extension here (coming soon).  

## ⚙ Spec

TypeFile must be in a UTF-8 encoded file.  
TypeFiles must always end in `.tf`.  
TypeFile is case-sensitive.  
TypeFile will ignore all whitespace (Tabs and spaces) except for inside a string.  
The proper [MIME](https://en.wikipedia.org/wiki/Media_type) type for TypeFile is `text/typefile`.

## 📕 Docs

These are the official docs for TypeFile. There is currently no syntax highlighting as [MarkDown](https://www.markdownguide.org) does not include support for it yet. Note that anytime an item "must be on a line by itself", this excludes comments.

**Chapters:**

- [Comments](#comments)
- [Key/Value Pairs](#keyvalue-pairs)
  - [Keys](#keys)
  - [Values](#values)
- [Objects](#objects)
  - [The root object](#the-root-object)
  - [Subobjects](#subobjects)
  - [Inline objects](#inline-objects)
  - [Object arrays](#object-arrays)
- [Type interpretation](#type-interpretation)
- [Types](#types)

### 💬 Comments

Comments are defined by using the `#` character. Everything after the `#` character on that line will be a comment.

```tf
# Hi im a comment
key = "value" # Look at the key/value pair to the left of me!
```

To make a multi-line comment, use three `#`'s. After three `#`'s, the rest of the file will be a comment until the repeating three closing `#`'s appear. Not putting the last three `#`'s before the EOF (End of file) will result in an error.

```tf
### I span
    Over Multiple
    Lines ###

### I cause
    an error
```

### 🗝 Key/Value pairs

Keys are on the left side of the equals sign, while values are on the right. Type declaration is always after the values on the right and is initialized using the `@` symbol. The key, equals sign, value, and type declaration must be on the same line. Also, types must not be incorrect to the value or else the it will throw an error.

```tf
# "value" and "type" are not valid and would throw an error.
# They are only used as an example for placement.
key = value@type
```

Since whitespace is ignored, the TFP (TypeFile Parser) sees the above data as:

```tf
key=value@type
```

So you can style it any way you want. My preferred styling is:

```tf
key = value @ type
```

Here is a small example with real values and types:

```tf
name = "zahtec" @ string
integer = 1 @ integer
```

Here is the same data but in [JSON](https://www.json.org):

```json
{
    "name": "zahtec",
    "integer": 1
}
```

Obviously, [JSON](https://www.json.org) does not support type declaration.

Blank values/keys are disallowed. This includes empty quoted keys, which you will learn about in the next section.

```tf
# Remember that "value" isn't a real value.
key =
 = value
 "" = value
 # All above data throws errors.
 ```

For most examples in these docs there will be no type declaration, you will learn about all the supported types, how to define them, and how they are interpreted later.

### 🔑 Keys

A key may either be bare or quoted.  
Bare keys may only contain ASCII letters, ASCII digits, underscores, and dashes (`A-Za-z0-9_-`), and should always be typed in [Camel Case](https://en.wikipedia.org/wiki/Camel_case).

```tf
key = "value"
cool-bare_key = "value"
1234 = "value"
"quoted key" = "value"
```

Quoted keys allow you to use a much broader amount of characters. Including whitespace, although using whitespace is discouraged.

```tf
"white space" = "value" # Okay but discouraged, use "white_space" or "white-space" instead.
"ʎǝʞ" = "value"
"key" = "value"
"$" = "USD"
```

Defining a key multiple times is disallowed. Bare keys and quoted keys are equivalent for this matter.

```tf
key = "value"
key = "value2" # Throws an error, duplicate key.
"key" = "value3" # And so does this.
```

### 🔢 Values

Values can be any one of these types:

- [String](#string)
- [Integer](#integer)
- [Float](#float)
- [Boolean](#boolean)
- [Array](#array)
- [Offset Date-Time](#offset-date-time)
- [Local Date-Time](#local-date-time)
- [Local Date](#local-date)

For strings, you can use single quotes (`'`) or double quotes (`"`). When using single quotes, you can nest double quotes inside of that string, and vice-versa. Two of the same type of quotes must be at the start and end of a string.

```tf
key = 'value'
key2 = "value"
nested = '"quote" -Zahtec'
# This throws an error.
key3 = 'value"
```

To declare a type for a value, use an `@` symbol. Everything after the `@` will be treated as a type.

```tf
key = "value" @ string
key2 = 1 @ integer
key3 = false @ boolean
# Most of this is not needed as it will be interpreted.
# You will learn about type interpretation later.
```

### 🔗 Objects

Objects are collections of key/value pairs known as properties. They are defined by headers, with square brackets on either side of their name like so: `[object]`. Object headers must always have a name. Their names follow the same syntax as keys. Objects must always be on a line by themselves and should always be typed in [Camel Case](https://en.wikipedia.org/wiki/Camel_case).

```tf
# Example object header.
[owner]
name = "zahtec" # Example object property.
```

Under the object header, and until the EOF (End of file) or next object header, are either the properties (key/value pairs) of that object or its subobjects. You will learn about subobjects later.

```tf
[object]
prop = "value"

[object2]
prop = "value"
```

As you may have noticed, there are duplicate properties (keys) in the file above, which is disallowed. Or so it may seem. Let me show you:

```tf
### The key with "value2" throws an error, since it has the same name as 
the one above, and they are both in the same object. ###
[object]
prop = "value"
prop = "value2"
```

But the same property across multiple objects is permitted. Properties inside different objects, even if they are named the same, do not conflict.

```tf
# This property is in the root object (Which you will learn about next).
prop = "value"

[example]
# While this one is in the "example" object.
prop = "value"
# They do not conflict since they are in a different object.
```

To make it clearer, here is the same data but in [JSON](https://www.json.org):

```json
{
    "prop": "value",
    "example": {
        "prop": "value"
    }
}
```

Whitespace around the object name is ignored. However, it is discouraged and best practice is to not use any extra, unneeded whitespace.

```tf
[a b c]
[ d e f ]
[ g        h            i ]
# None of the above names will have their whitespace saved.
# While this quoted one will.
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

#### 🌱 The Root Object

The root object is the main object the file is represented as. All root object properties must be before the first non-root object or EOF. Most of the time root object properties are called keys.

```tf
# This key (property) is in the root object.
key = "value"

[object]
prop = "value" # This key/property is not.
```

The root object does not support subobjects. If you try to, it will throw an error.

```tf
# This will throw an error.
:[suboobject]
prop = "value"
```

But what are subobjects and how do I use them correctly? To learn, continue onto the next section.
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
#### Object arrays
Object arrays allow for multiple, unnamed objects, to be inside a named array. They can be expressed by using an object header with double brackets. Like so: `[[array]]`. The first instance of that header defines the array and its first element, each subsequent instance creates and defines a new object element in that array. The objects are inserted into the array in the order they were encountered. Object arrays follow all the same concepts that a regular object does except for duplicate naming, this will instead add a new object to the array.
```
# These are all valid.
[[array]]
prop = "value"
[[array]]
prop = "value2"
# Duplicate names are allowed for array headers as that is how you add a new object to the array.
```
Here is the same data but in [JSON](https://www.json.org):
```
{
	"array": [
		{ "prop": "value" },
		{ "prop": "value2" }
	]
}
```
Empty objects within the array are dissalowed.
```
# This throws an error.
[[array]]

[object]
prop = "value"
```
Object arrays can be quoted or bare, like keys.
```
# This is valid.
[["I am quoted"]]
prop = "value"
```
Object arrays count as object headers.
```
# These are all valid.
[object]
prop = "value"
[[array]]
prop = "value2"
```
Subobjects, inline objects, and Subobjectarrays work as well. Like so:
```
# These are all valid.
[[array]]
prop = "value"
[[array]]
prop = "value2"
prop2 = { inline = "value" }
:[subobject]
prop = "value"
:[[subobjectarray]]
prop = "value"
```
Here is the same data but in [JSON](https://www.json.org):
```
{
	"array": [
		{ "prop": "value" },
		{
			"prop": "value2",
			"prop2": { "inline": "value" },
			"subobject": { "prop": "value" },
			"subobjectarray": [
				{ "prop": "value" }
			]
		}
	]
}
```
An array of inline objects is not equal to an object array (even if you can produce the same result) and will throw an error.
```
# This throws an error.
prop = [{ prop = "value" }]

[[prop]]
prop = "value"
```
### Type interpretation
Type interpretation will only go as far as the basic type. This applies for all types.
```
# This is interpreted as a string.
prop = 'value'
# This is interpreted as an integer. Not a integer-decimal-positive.
prop2 = 2
# This is not needed since it is automatically interpreted.
prop3 = false @ boolean
```
### Types
- String
- Integer
- Float
- Boolean
- Array
- Offset Date-Time
- Local Date-Time
- Local Date
- Any
- Unknown
