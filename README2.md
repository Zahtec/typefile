# TypeFile ![version 1.0](https://img.shields.io/badge/version-1.0-blue)

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
- [Arrays](#arrays)
- [Type interpretation](#type-interpretation)
- [Type Declaration](#type-declaration)
- [Types](#types)
  - [String](#string)
  - [Integer](#integer)
  - [Float](#float)
  - [Boolean](#boolean)
  - [Array](#array)
  - [Any](#any)

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
- [Any](#any)

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

Like keys, empty objects and object names are disallowed and duplicate objects are disallowed aswell. Empty objects are objects with no properties or subobjects until the EOF or next object header.

```tf
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

The root object does not support subobjects. If you try to, it will throw an error. You will learn about subobjects in the next section

```tf
# This will throw an error.
:[suboobject]
prop = "value"
```

#### ⏬ Subobjects

Subobjects are objects within objects/subobjects. This can go on infinitely. Subobjects are defined by putting an `:` before an object header and follow all the same core concepts that a regular object does. The count of how many `:` characters come before the header is based on how deep that subobject is.

```tf
[object]
prop = "value"
:[subobject]
prop = "value" # Accessed using object.subobject.prop
::[subobject2]
prop = "value" # Accessed using object.subobject.subobject2.prop
```

For readability, since whitespace is ignored, you may style it however you like. Like so:

```tf
[object]
prop = "value"
    :[subobject]
        prop = "value"
        ::[subobject2]
            prop = "value"
```

To make more sense of this, here is the same data but in [JSON](https://www.json.org):

```json
{
    "object": {
        "prop": "value",
        "subobject": {
            "prop": "value",
            "subobject2": {
                "prop": "value"
            }
        }
    }
}
```

The amount of `:` characters before your subject depends on how deep it is. A subobjects "deepness" means how many subobjects are between and its main parent object. This count includes the parent object. For example, below, the subobject "depth-1" is inside of 1 object, its main parent "depth-test". This means it is only 1 deep. While the subobject "depth-2" is inside of 2 objects, its main parent "depth-test", and "subobject-depth-1".

```tf
# These are all valid.
[depth-test]
prop = "value"
:[depth-1]
prop = "value"
::[depth-2] # Accessed using depth-test.depth-1.depth-2
prop = "value"
:[deep-1]
prop = "value"
```

To make sense of this all, here is the same data but in [JSON](https://www.json.org):

```json
{
    "depth-test": {
        "prop": "value",
        "depth-1": {
            "prop": "value",
            "depth-2": {
                "prop": "value"
            }
        },
        "deep-1": {
            "prop": "value"
        }
    }
}
```

This syntax make look weird and repetitive, but really you shouldn't be making so many subobjects you end up with something like `::::::::[subobject]`. That should tell you that you need to structure your data storage better. Also, its very easy to read. Let's say you are looking for a particular subobject, you are always going to be looking for headers with 1 or more `:` character infront of it.

#### 📏 Inline objects

Inline objects provide a more compact syntax for defining objects. Inline objects are defined by inline characters `{` and `}` in a value position for a property (key). They follow the same rules as regular objects. Within the `{` and `}` characters, properties are defined using one or more comma separated key/value pairs. Inline objects must always be inline and can not be spread across multiple lines. Subobjects within inline objects do not use the traditional `:` character. Just create a property with another inline object.

```tf
# Example of an inline object.
object = { prop = "value", prop2 = "value" }
```

This is the same as:

```tf
[object]
prop = "value"
prop2 = "value"
```

Subobjects within inline objects are defined like so:

```tf
inline = { prop = "value", subobject = { prop = "value" } }
```

This is the same as:

```tf
# These are all valid.
[inline]
prop = "value"
:[subobject]
prop = "value"
```

Inline objects follow the same rules as regular objects. So, you cant define the same object twice, etc.

```tf
inline = { prop = "value" }

[inline] # This will throw an error.
prop = "value"
```

Inline objects are slightly different when it comes to type declaration, since they are inline. Regular type declaration with non-inline objects would look like so:

```tf
[owner]
name = "zahtec" @ string
ID = 1234 @ integer
```

While an inline object would use `&` symbols to declare types for each comma separated property, in order. Like so:

```tf
owner = { name = "zahtec", ID = 1234 } @ string & integer
```

Remember since whitespace is ignored I am using my own style, but you can style this any way you prefer as long as its on the same line.

#### 🛅 Object arrays

Object arrays allow for multiple, unnamed objects, to be inside a named array. They can be defined by using an object header with double `[]` characters, like so: `[[array]]`. The first instance of that header defines the array and its first element, each subsequent instance creates and defines a new unnamed object in that array. Once the next object header is hit, the array can no longer be added to. The objects are inserted into the array in the order they were encountered. Object arrays follow all the same concepts that a regular object does, except for duplicate naming and subobject support. Duplicate naming would usually throw an error, this will instead add a new object to the array. As for subobjects, they are disallowed since object arrays are meant for storing unnamed objects.

```tf
[[array]]
prop = "value"
[[array]]
prop = "value2"
```

For clarity, here is the same data but in [JSON](https://www.json.org):

```json
{
    "array": [
        { "prop": "value" },
        { "prop": "value2" }
    ]
}
```

Just like regular objects, empty objects within the array are disallowed.

```tf
[[array]]
prop = "value"

[[array]] # This throws an error.

```

Object arrays can be quoted or bare, like regular objects.

```tf
[["I am quoted"]]
prop = "value"
```

Object arrays can only be added to if they are uninterrupted from the first, defining, header.

```tf
[[array]]
prop = "value"

[object]
prop = "value"

[[array]] # This throws an error. The object "object" is interrupting.
prop = "value"
```

An array of inline objects is not equal to an object array (even if you can produce the same result) and will throw an error.

```tf
prop = [{ prop = "value" }]

[[prop]] # You cannot add to the array using this method, this will throw a duplicate name error.
prop = "value"
```

Object arrays do not support subobjects as they weren't meant for it, they will instead throw an error.

```tf
[[array]]
prop = "value"
:[subobject] # This will throw an error.
prop = "value"
```

Although this is not vice-versa. Object arrays can be a sort of "subobject array" when inside a regular object. If inside eachother though, this will not work.

```tf
[object]
prop = "value"
:[[subobjectarray]]
prop = "value"
# Just like regular object arrays, to add to it, you will need it to be uninterrupted. And in this case, at the same depth.
# ::[[subobjectarray]] This would throw an error as you are trying to add a subobject array into a subobject array.
:[subobject]
prop = "value"
# :[[subobjectarray]] This would throw an error since the subobject "subobject" interrupts it.
```

And to sum it all up, here is the above data in [JSON](https://www.json.org):

```json
{
    "object": {
        "prop": "value",
        "subobjectarray": [
            { "prop": "value" }
        ],
        "subobject": {
            "prop": "value"
        }
    }
}
```

### 💼 Arrays

Arrays are collections of multiple values separated by commas inside `[` and `]`. As mentioned before, you can make objects inside arrays, but they are not equivalent to object arrays. Values inside of them can be any type that a value in a key/value (property) can be.

```tf
array = [ "value", "value2" ]
```

Whitespace is ignored as always, so you can style it however you want.

```tf
# You can style it like this.
array = ["value1","value2"]
# Or this.
array2 = [ "value", "value2" ]
```

Array type declaration is just like an inline object. Types separated by `&` symbols for each value inside the array, in order.

```tf
array = [ "value1", 1 ] @ string & integer
```

### ❓ Type interpretation

Type interpretation will only go as far as the basic type. This applies for all values/types. You will learn about basic types vs specific/advanced types in the next section.

```tf
# This is interpreted as a string.
prop = 'value'
# This is interpreted as an integer. Not a integer-decimal-positive.
prop2 = 2
# This is not needed since it is automatically interpreted as a boolean.
prop3 = false @ boolean
```

### 📣 Type Declaration

Each type has its own main declaration and specifics separated by `.` symbols. Each type i will have its own "Declaration" section which shows how to declare it and all its possible specifics. Specifics are TypeFiles word for subtypes.

```tf
# String declaration. Simple, no specifics
prop = "value" @ string
# Integer declaration. Has many specifics.
prop2 = 1 @ integer.positive
# This means that when writing to the file, the above property can only be a positive integer.
```

### 🎫 Types

These are all the types that TypeFile supports. How they are parsed will depend on the language you are using. For example, if you are using [Javascript](https://javascript.com), integers and floats will just be basic numbers.

- String
- Integer
- Float
- Boolean
- Array
- Any

#### String

.. Continue with types ..