# TypeFile ![Version 1.0](https://img.shields.io/badge/version-1.0-blue)

TypeFile is a strongly typed configuration file format for the modern age. It currently supports JavaScript/TypeScript only, but with community help and as time passes I hope for it to be supported by 100+ languages. TypeFile has a beautiful syntax thats easy to read and write, along with a fast parser and docs you can actually read. Versions (standards) of TypeFile follow minified [Semantic Versioning](https://semver.org), MAJOR.MINOR.

## ℹ How do I use it?

To start using TypeFile, you need a parser referred to as the TFP (TypeFile Parser). The TFP for your language will read and write data to your TypeFile (`.tyf`) in the correct manner. The TFP will throw an error if you try to read a a file that doesn't end in `.tyf`. This is optional, but recommended: You can also download a TypeFile plugin for your preferred [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment). It will automatically check your syntax inside of `.tyf` files, and methods inside of your preferred programming languages files. It also adds syntax highlighting for `.tyf` files. Anyone who wants to contribute and make a parser for a specific language or create an IDE extension is welcome to do so, learn more about contributing in the [🔌 Contributing](#-contributing) section. Packages/extensions are separated onto separate branches of this repository each with their own respective name, this is our way of organizing OSS all in one place.

## 📦 Packages

**― NodeJS ―**  

[https://nodejs.org](https://nodejs.org)

```shell
npm install typefile
```

The NodeJS TFP enhances JS numbers and will enable the float and integer type work properly (if enabled).  
Branch: [node](:node)  
Author: [Zahtec](https://github.com/Zahtec)  

**― Deno ―**  

[https://deno.land](https://deno.land)

```ts
import * as typefile from 'https://deno.land/x/typefile@1.0.0/mod.ts'
```

The Deno TFP has great functionality with all types thanks to Denos native support for [TypeScript](https://www.typescriptlang.org).  
Branch: [deno](:deno)  
Author: [Zahtec](https://github.com/Zahtec)  

All packages follow the [Semantic Versioning](https://semver.org) standard.

## ➕ Extensions

**― Visual Studio Code ―**  

[https://code.visualstudio.com](https://code.visualstudio.com)

[https://marketplace.visualstudio.com/items?itemName=zahtec.typefile](https://marketplace.visualstudio.com/items?itemName=zahtec.typefile)

The Visual Studio Code TypeFile extension adds syntax highlighting for `.tyf` files and more.  
Branch: [vsc](:vsc)  
Author: [Zahtec](https://github.com/Zahtec)  

All extensions follow the [Semantic Versioning](https://semver.org) standard.

## 🔌 Contributing

Contributing to TypeFile is greatly appreciated. There are some standards to go by before you contribute though. Lets go over them.

**#1:** Issues on the [main](:main) branch (this one) must be only related to updating the docs or updating the standard (this includes the syntax guide for packages). Any issues you may have or help you need for packages/extensions are strictly tied to its (that packages/extensions) branch. You can find a list of packages with their respective branches in the [📦 Packages](#-packages) section. And for extensions, you can find a list of them with their respective branches in the [➕ Extensions](#-extensions) section.

**#2:** When making a new parser for TypeFile in a language thats already been covered, it must have a significant advantage or wanted change to the last package for that language. You can release this parser on your own profile if you wish.

**#3:** When making a new TypeFile IDE extension for one thats already been covered, it must have a significant advantage or wanted change to the last extensions for that IDE. Changing the colors does not count, since you can do this by changing settings for already made packages. You can release the extension on your own profile if you wish.

**#3:** When making a new TypeFile parser you must follow the syntax guide. This is to ensure all parsers feel the same and familiar so switching from language to language isn't difficult.

**#4:** When making a new TypeFile extension for an IDE, it must follow our style guide. This outlines how text is meant to be highlighted.

**#5:** All packages/extensions must follow the [Semantic Versioning](https://semver.org) standard.

Now lets get onto how to contribute with new packages/extensions. The template branch for each one (package/extension) contains most of the info on how to get started, so these steps will be very simple.

1. Go to the [package](:package-template)/[extension](:package-extension) template branch.
2. Fork it
3. Make your parser/extension following our syntax/style guide.
4. Make a pull request on the [main branch](:main) (this one) with your repository.
5. I will review it, ask you some questions if possible, and make my decision wether to add it or not.

When updating your package/extension, you will have to submit a new pull request on your branch, do not submit updates on the main branch.

## ⚙ Spec

- TypeFiles must always end in `.tyf`.
- TypeFiles must be encoded in [UTF-8](https://en.wikipedia.org/wiki/UTF-8).
- TypeFile is case sensitive.
- TypeFile supports [CRLF and LF](https://developer.mozilla.org/en-US/docs/Glossary/CRLF). But LF is preferred.
- TypeFile ignores all whitespace between defining characters.
- TypeFiles official MIME type is `text/typefile`.

## 📕 Docs

This is the official documentation for TypeFile 1.0. It outlines the core concepts of TypeFile and sets a standard. If you are looking to make a package, go to the [🔌 Contributing](#-contributing) section. There is currently no syntax highlighting as [MarkDown](https://www.markdownguide.org) does not include support for it yet. Note that anytime an item "must be on a line by itself", this excludes comments.

**― Chapters ―**  

- [▶ Getting Started](#-getting-started)
- [💬 Comments](#-comments)
- [🗝 Key/Value Pairs](#-key-value-pairs)
- [🔗 Objects](#-objects)
  - [🌱 The Root Object](#-the-root-object)
  - [⏬ Subobjects](#-subobjects)
  - [🖇 Object Arrays](#-object-arrays)
- [🔢 Values](#-values)
  - [📏 Inline Objects](#-inline-objects)
  - [📃 Arrays](#-arrays)
  - [🧵 Strings](#-strings)
  - [1️⃣ Integers](#-integers)
  - [⚓ Floats](#-floats)
  - [⛔ Booleans](#-booleans)
- [🎫 Types](#-types)
  - [📣 Type Declaration](#-type-declaration)
  - [❓ Type Interpretation](#-type-interpretation)
  - [📃 Arrays](#1-arrays)
  - [🧵 Strings](#1-strings)
  - [1️⃣ Integers](#1-integers)
  - [⚓ Floats](#1-floats)
  - [⛔ Booleans](#1-booleans)
  - [🌌 Any](#-any)

The reason you see duplicate chapters (2 strings, floats, etc...) is because of how the docs are split up. Under the [🔢 Values](#-values) chapter there is each type that can be a value plus its unique features. Under the [🎫 Types](#-types) chapter, you will learn about type declaration in general and each type and how to declare it.

### ▶ Getting Started

To get started with TypeFile, open an IDE, for example: [Visual Studio Code](https://code.visualstudio.com). If your IDE is support, find the extension for it in the [➕ Extensions](#-extensions) section and install it. Make a new project and create a `tutorial.tyf` file. Now as you read the docs, you can follow along and keep track of what you learned for reference later. If you want to try out using a TFP (TypeFile Parser) for your preferred language at the same time, find the corresponding one for your language in [📦 Packages](#-packages) and follow its specific docs alongside.

A lot of the time these docs will show corresponding data in [JSON](https://www.json.org). This is to help people transitioning over and clarifies many things.

### 💬 Comments

Comments are defined by using the `#` (pound) character. Everything after the pound on that line will be a comment.

```tyf
# Hi im a comment
key = "value" # Look at the key/value pair to the left of me!
```

To make a multi-line comment, use three pound signs. After the three pound signs, the rest of the file will be a comment until the corresponding three pound signs appear, which will close the comment (end it).

```tyf
###
I span
Over Multiple
Lines
###

### So
    Do
    I ###
```

Not putting the last three pound signs before the EOF will throw an error.

```tyf
###
I cause
an error
```

Whitespace is ignored around all declaring characters, as laid out in [⚙ Spec](#-spec). This applies especially for multi-line comments. Here is an example:

```tyf
# # # This comment is still a one line comment, since it is a comment with 2 pound signs at the beginning.
###
This is a multi line comment since the declaring characters 
(the 3 pound signs) 
are not spaced out
###
```

That first comment is seen as: `# # This comment is...` instead of being a multi-line comment.

### 🗝 Key/Value Pairs

**Note:** Key/Value pairs are known as properties in TypeFile.

Keys are on the left side of the equals sign, while values are on the right. The key, equals sign, and value must be on the same line.

```tyf
# For this example this key is named "key", and the value is a string.
key = "value"
# Since keys are known as properties in TypeFile, examples in these docs will most of the time look like this:
prop = "value"
```

Above data in [JSON](https://www.json.org):

```json
{
    "key": "value",
    "prop": "value"
}
```

Keys have 2 types. A bare key or a quoted a key. Bare keys may only contain ASCII letters, ASCII digits, underscores, and dashes (`A-Za-z0-9_-`). Quoted keys on the other hand allow for a much broader range of characters to be used; Unicode characters and whitespace.

```tyf
im-bare = true
"im quoted" = true
"😃" = "why" # Valid but discouraged, don't use emoji for key names.
# im bare = true
# If added, the above key would throw an error since whitespace isn't supported by bare keys.
```

Blank values/keys are disallowed. This includes empty quoted keys.

```tyf
# These all throw errors.
key =
 = "value"
 "" = "value"
 ```

Duplicate keys are disallowed and will throw an error. This includes both quoted and bare keys, they are equivalent for this matter.

```tyf
key = "value"
# key = "value"
# If added, the above key would throw an error for duplicate naming.
# "key" = "value"
# ^^ Same goes for this one
```

Keys should almost always follow [Camel Case](https://en.wikipedia.org/wiki/Camel_case) typing.

```tyf
camelCase = true
```

Whitespace is ignored around all declaring characters, as laid out in [⚙ Spec](#-spec). Declaring characters for properties include the property name and its value (key/value).

```tyf
k e y = value
# The above property would throw an error since bare keys do not support whitespace.
key = 1 2 4
# The above property would also throw an error since the value is spaced out when it shouldn't.
```

Otherwise, whitespace can be used to style things that aren't declaring characters. Like so:

```tyf
# These are both valid.
prop="value"
prop2  =  "value"
```

### 🔗 Objects

Objects are collections of properties. They are defined by headers, with `[` and `]` (square brackets) on either side of their name, like so: `[object]`. Object headers must always have a name. Their names follow the same syntax as keys, so they can be bare or quoted and should almost always follow [Camel Case](https://en.wikipedia.org/wiki/Camel_case) typing.

```tyf
# Example object header.
[object]
prop = "value"
```

Object headers follow the naming standard as properties do. They can be quoted or bare, and there must be no duplicates. Just like property names, they should almost always follow [Camel Case](https://en.wikipedia.org/wiki/Camel_case) typing.

```tyf
[bare]
prop = "value"

["quoted"]
prop = "value

# [bare]
# If added, the above object header would throw an error for duplicate naming.
```

Empty objects (objects with no properties, like a property without a value) and object names are disallowed. If a key and object/[subobject](#-subobjects) are under the same parent object (including [the root object](#-the-root-object)) and they both have the same name, an error is thrown.

```tyf
# These all throw an error for empty names.
[""]
prop = "value"
[]
prop = "value"

[empty] # This throws an error for being empty.


[object]
prop = "value"
[object] # Duplicate name! Error is thrown.
prop = "value2"
```

Under the object header, and until the EOF or next object/[object array](#-object-arrays) header, are either the properties of that object or its [subobjects](#-subobjects).

```tyf
prop = "value"

[object]
prop = "value"

[object2]
prop = "value"
```

In the example above, there is multiple properties with the same name. But since they properties are all in separate objects, they are not affected by eachother. The example below shows this in action with the commonly used dot notation format. For this example, the `file` variable is assigned to the root of our file ([🌱 The Root Object](#-the-root-object)).

```tyf
root = "value" # file.prop

[object]
prop = "value" # file.object.prop
# Since the "root" property is in the root, they do not affect eachother (since its counted as a separate object).

[object2]
prop = "value" # file.object2.prop
```

Above data in JSON:

```json
{
    "prop": "value",
    "object": {
        "prop": "value"
    },
    "object2": {
        "prop": "value"
    }
}
```

#### 🌱 The Root Object

The root object is the main object the file is represented as. All root object properties must be before the first non-root object. You can think of the root object as the first `{}` (curly brackets) in JSON. For example, lets say you read your file and assign it to a variable called `file` (this differs language to language based on package and is only used a basic example). When you point at a property just by doing `file.prop`, that property is in the root object. Here is an example with dot notation:

```tyf
prop = "value" # This property is in the root object. Dot notation: file.prop

[object]
prop = "value" # This key is inside the object named "object", so it's not in the root. Dot notation: file.object.prop
```

Same data as above but in JSON:

```json
{
    "prop": "value",
    "object": {
        "prop": "value"
    }
}
```

The root object does not support [subobjects](#-subobjects) and will throw an error.

```tyf
:[subobject] # This will throw an error.
prop = "value"
```

#### ⏬ Subobjects

Subobjects are a way to nest objects within objects. This can go on until the nesting limit for the TFP you are using (which depends on the language you are using). To find out the nesting limit for your particular parser, read the docs for it. Subobjects are defined by putting an `:` (colon) before an object header. They follow all the same core concepts that an object does. The count of how many colons come before the header is based on how deeply nested that subobject is. Here is an example with dot notation, with `file` being [the root object](#-the-root-object):

```tyf
prop = "value" # file.prop

[object]
prop = "value" # file.object.prop
:[subobject] # A depth of 1, it is nested inside the the object named "object"
prop = "value" # object.subobject.prop
::[subobject2] # A depth of 2, it is nested inside both the main object named "object", and the subobject named "subobject"
prop = "value" # object.subobject.subobject2.prop
```

That data above in JSON:

```json
{
    "prop": "value",
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

To clarify what a subobjects "depth" is, let me show an example. An objects depth tells the TFP what object that subobject is in. Since you can put subobjects inside other subobjects, these colons tell which object a subobject is supposed to be in. Once again I have added dot notation to help with this understanding. For this example, `file` is the variable that we have assigned to our file read. The one colon infront of the "ownership" subobject tells the TFP that this subobject is meant to be under the "owner" object, not the "details" subobject. As for the "typefile-info" subobject, its 2 colons tell the TFP that its meant to be inside the "ownership" subobject, same goes for the "json-info" subobject.

```tyf
[owner] # file.owner
:[details] # file.owner.details
name = "Zahtec"
:[ownership] # file.owner.ownership
typefile = true
json = false
::[typefile-info] # file.owner.ownership."typefile-info"
contributed = true
::[json-info] # file.owner.ownership."json-info"
contributed = false
```

Subobjects follow the same rules as regular objects. If a subobject has a duplicate at the same depth or the same name as a key inside the same object an error must be thrown.

```tyf
[object]
prop = "Don't duplicate me!"
#:[prop] If added, an error would be thrown.
:[subobject]
prop = "value"
#:[subobject] If added, an error would be thrown.
```

As said in the [🌱 The Root Object](#-the-root-object) section, subobjects are not supported in the root object. Even though the root object is considered an object, it is primitive in this context.

#### 🖇 Object Arrays

Object arrays allow for multiple unnamed objects to be inside a named array. They can be initialized by using an object header with double `[]` (square bracket) characters, like so: `[[array]]`. The first instance of that header defines the array and its first unnamed object, each subsequent instance creates and defines a new unnamed object in that array. Once the next object header is hit, the array can no longer be added to. The objects are inserted into the array in the order they were encountered. Object arrays follow the same core concepts as regular objects do, with a few exceptions. Subobjects are disallowed.

```tyf
[[array]] # Initializes array with name "array".
prop = "value" # Creates an unnamed object inside the array with this property.

[[array]] # Any properties below this will be added to the 2nd unnamed object.
prop = "value2"

[object]
prop = "value"

# [[array]] If added, an error would be thrown for duplicated naming.
```

Same data as above but in JSON:

```json
{
    "array": [
        { "prop": "value" },
        { "prop": "value2" }
    ]
}
```

### 🔢 Values

Values are whats on the right of the `=` for a property. They each have their own special traits which you will learn about now. Here a list of values:

- [📏 Inline Objects](#-inline-objects)
- [📃 Arrays](#-arrays)
- [🧵 Strings](#-strings)
- [1️⃣ Integers](#-integers)
- [⚓ Floats](#-floats)
- [⛔ Booleans](#-booleans)