# TypeFile
!! **THIS IS A CONCEPT** !!  
A type-oriented configuration file similar to [TOML](https://github.com/toml-lang/toml). Built for languages with strict typing like [TypeScript](https://www.typescriptlang.org). In JavaScript/TypeScript all numbers have 1 type, the node package handles this. For other languages like [Java](https://www.java.com), types like `decimal` or `float` will work.

## ❔ So what is TypeFile?
TypeFile aims to be a strictly typed configuration/data storage file. It is very similar to [TOML](https://github.com/toml-lang/toml).

## ⌨ How do I use it?
To get started with TypeFile, you need the Type File Interpreter or TFI

If you use [VSC](https://code.visualstudio.com), install the syntax extension here (coming soon...).  
For [Node.js](https://nodejs.org): Get the [npm](https://www.npmjs.com) package here (coming soon...).  
For [Deno](https://deno.land): Get the [npm](https://www.npmjs.com) package here (coming soon...).  
For [Python](https://www.python.org): Get the [pip](https://pip.pypa.io) package here (coming soon...).  
For [Rust](https://www.rust-lang.org): Get the [cargo](https://crates.io) crate here (coming soon...).  

**Disclaimer: Since this project is brand new, only the services listed above currently have support**.

## ⚙ Spec
TypeFile is case-sensitive.  
A `.tf` or `.typefile` file must be valid UTF-8 encoded Unicode document.

## 📕 Docs

### Comments
```toml
# Comments
#* Multi
   Line
   Comments *#
```
*# means the rest of the line is a comment. #\* means anything after it before \*# is a comment*  

### Key/Value Pairs
Keys are on the left side of the equals sign, while values are on the right. Type declaration is always after the values on the right. Whitespace is ignored around key names and values. The key, equals sign, value, and type declaration must be on the same line. Also, types must not be incorrect to the value.
```toml
# Keep in mind "type" and "value" are not a real types/valid values, and are only used as an example
key = value<type>
# Small Example
key = 'string'<string> # Okay
key = 'string'<number> # ERROR
# These type declarations are both useless as they would be automatically interpreted
```
**Since whitespace is ignored, this is rendered as:**
```toml
key=value<type>
```
**Unspecified values are dissalowed. This does not include empty strings or null/undefined**
```toml
key = # ERROR
key2 = "" # Okay but discouraged. Set to string type and use null instead
key3 = null # Okay
key4 = undefined # Okay
```
### Keys
A key may either be bare, quoted, or dotted.
Bare keys may only contain ASCII letters, ASCII digits, underscores, and dashes `(A-Za-z0-9_-)` and should always be typed in [Camel Case](https://en.wikipedia.org/wiki/Camel_case). Note that bare keys are allowed to be composed of only ASCII digits, e.g. 1234, but are always interpreted as strings.
```toml
key = "value"
bare_key = "value"
bare-key = "value"
1234 = "value"
```
Quoted keys allow you to use a much broader amount of characters. Including special characters and whitespace, although using whitespace is discouraged. Not that although bare keys dont have quotes and quoted keys do. A key is always interpreted as a string in most languages.
```toml
"character encoding" = "value" # Okay but discouraged, use "character_encoding" instead
"ʎǝʞ" = 'value'
'key2' = "value"
'quoted "value"' = "value"
"100$" = "broke"
```
A bare key must be non-empty and so must a quoted key. Unlike [TOML](https://github.com/toml-lang/toml).
```toml
= "no key name"  # ERROR
"" = "blank"     # ERROR
'' = 'blank'     # ERROR
```
Dotted keys are a sequence of bare or quoted keys joined with a dot. This allows for grouping similar properties together. Whitespace around dotted keys are ignored. Note that dotted keys make any value before the last a table, and directly defining those will throw an error.
```toml
name = "Orange"
physical.color = "orange"
physical.shape = "round"
price."$" = true
white.       space = false
```
[JSON](https://www.json.org) conversion:
```json
{
  "name": "Orange",
  "physical": {
    "color": "orange",
    "shape": "round"
  },
  "price": {
    "$": true
  }
}
```
Defining a key multiple times is dissalowed.
```toml
# ERROR
key = 'value'
key = 'vaue2'
```
Note that bare keys and quoted keys are equivalent:
```toml
# ERROR
spelling = "favorite"
"spelling" = "favourite"
```
As long as a key hasn't been directly defined, you may still write to it and to names within it.
```toml
# This makes the key "fruit" into a table.
fruit.apple.smooth = true

# So then you can add to the table "fruit" like so:
fruit.orange = 2

# As mentioned earlier, dotted keys make any value before the last a table, and directly defining those will throw an error.
fruit = "orange" # ERROR
fruit.apple = true # ERROR
```
```toml
# This defines the value of fruit.apple to be an integer.
fruit.apple = 1
# So the table "fruit" has a value "apple" and its set to 1.
# Since "fruit" is now a table, and "apple" is defined as an integer. You cant act like "apple" is a table
fruit.apple.smooth = true # ERROR
```
Since bare keys can be composed of only ASCII integers, it is possible to write dotted keys that look like floats but are 2-part dotted keys. This is **HIGHLY** discouraged as it goes directly against strict typing.
```toml
3.14159 = "pi"
```
[JSON](https://www.json.org) conversion:
```json
{ "3": { "14159": "pi" } }
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
- Inline Table/Object

For strings, you can use single quotes (`'`) or double quotes (`"`).
```toml
key = "value"
key2 = 'value2'
```
Declaring types on values is very simple and readable, and takes the form of a generic. They are case-sensitive and must always be lowercase.
```toml
number = "1"<string> # Accurate but useless, TypeFile automatically interprets this as a string type based of the value
```
Union types allow for multiple types fused together.
```toml
number = "1"<string|integer> # Not useless, allows code to overwrite this value to any integer instead of just a string
```
Any types allow for any type to be written to the value. But is discouraged due it removing most strict typing.
```toml
any = 'string'! # the ! declares it as an any type, this must be on the same line right after the value
example         =    'string'           ! # since all whitespace is ignored, this would work but is discouraged
```
Unknown types allow for ... (fill in after experimenting)
```toml
unknown = null? # the ? declares it as an unknown type
```
You can put this at the end of any value, but it is most commonly used at the end of undefined or null, and is discouraged otherwise
```toml
unknown = 'string'? # Okay, but discouraged
example = undefined? # Okay, you might not know the data that will be overwritten here
```
### Types
This lists out all the types in TypeFile and there rules. Some things may very depending on the programming language you decide to use TypeFile with.
#### Interpretation
Type interpretation will only go as far as the basic type.
```toml
int = 1 # This will interpret the type as a integer, not integer-decimal-positive
example = 1.0<float<fraction>> # This is a float fraction. It will not continue to be interpreted as a positive
```
#### Strings
Strings are UTF-8 encoded (required for TypeFile). They can contain any amount of text and whitespace will be saved. They can also span multiple lines and use double quotes (`"`) or single quotes (`'`).
```toml
string = 'string'
multiline = '''this string
spans over
multiple lines'''
double = "quotes"
work = """as
well"""
```
You can represent any string using the string type.
```toml
string = 'hello'<string> # Okay
str = 'world'<string> # Okay
s = "I <3 strings"<string> # Okay
```
For even stricter typing, you can use generics.
```toml
single = 'quotes'<string<single>> # Okay
double = "quotes"<string<double>> # Okay
example = "hello"<string<single>> # ERROR
```
#### Integers
Unlike [TOML](https://github.com/toml-lang/toml), TypeFile does not require a `+` infront of positive integers, but does require a `-` infront of negative integers.
```toml
integer = 1 # Okay, positive integer 1
int = +1 # Okay, but discouraged. For cleaner code remove the +
i = -1 # Okay, negative integer 1
```
Leading zeros are not allowed. The integer value `-0` is valid and identical to an unprefixed zero.
```toml
int = 0100 # ERROR
zero = -0 # Okay, but does not change the outcome
normal = 0 # Okay
```
For large numbers, you may use underscores between digits to enhance readability. This is supported for all number types. Each underscore must be surrounded by at least one digit on each side.
```toml
integer = 1_000 # Is the same as the readable 1,000. Or the less readable 1000
int = 5_349_221
why = 1_2_3_4_5  # Okay, but discouraged. Is equal to 12345 or 12,345
```
Non-negative integer values may also be expressed in hexadecimal, octal, or binary. In these formats, leading zeros are allowed (after the prefix). Hex values are case-insensitive and underscores are allowed between digits (but not between the prefix and the value).
```toml
# hexadecimal with prefix `0x`
hexadecimal = 0xDEADBEEF
hex = 0xdeadbeef
h = 0xdead_beef

# octal with prefix `0o`
octal = 0o01234567
oct = 0o755

# binary with prefix `0b`
binary = 0b11010110
```
Arbitrary 64-bit signed integers (from −2^63 to 2^63−1) should be accepted and handled losslessly. If an integer cannot be represented losslessly, an error must be thrown. ***(message from TypeFile dev - I have no clue what this means)***
You can represent any integer using the integer type.
```toml
integer = 1<integer> # Okay
int = 1_000<integer> # Okay
hex = 0xDEADBEEF<integer> # Okay
oct = 0o755<integer> # Okay
bin = 0b11010110<integer> # Okay
```
For even stricter typing, you can use generics.
```toml
integer = 1<integer<decimal>> # Okay
int = -1<integer<decimal>> # Okay
i = 0<integer<zero>> # Okay
hex = 0xDEADBEEF<integer<hexadecimal>> # Okay
oct = 0o755<integer<octal>> # Okay
bin = 0b11010110<integer<binary>> # Okay
binary = 1<integer<binary>> # ERROR
```
You can even do positive/negative typing like so:
```toml
integer = 1<integer<decimal<positive>>> # Okay
int = -1<integer<decimal<negative>>> # Okay
why = 0<integer<zero<negative>>> # Okay, but discouraged
hex = 0xDEADBEEF<integer<hexadecimal<positive>>> # Okay
h = -0xDEADBEEF<integer<hexadecimal<negative>>> # Okay
# And so on
```
#### Floats
Floats should be implemented as IEEE 754 binary64 values. ***(message from TypeFile dev - I have no clue what this means)***

A float consists of an decimal integer part (with allowed leading zeros) followed by a fractional part and/or an exponent part. If both a fractional part and exponent part are present, the fractional part must precede the exponent part.
```toml
# fractional
float = 1.0 # Okay. Could be used to represent as a float instead of a integer-decimal without declaring it
flt = 3.1415
f = -0.01

# exponent
flt2 = 5e+22 # Same as 5⋅10^22
flt3 = 1e06
flt4 = -2E-2

# both
flt5 = 6.626e-34
```
A fractional part is a decimal point followed by one or more digits. An exponent part is an E (upper or lower case) followed by an integer part. The decimal point, if used, must be surrounded by at least one digit on each side.
```toml
float = .7 # ERROR
flt = 7. # ERROR
f = 3.e+20 # ERROR
```
As mentioned before, you can use underscores for readability on all number types.
```toml
float = 1_000.1 # Is the same as the readable 1,000.1. Or the less readable 1000.1
flt = 2_0e+22 # Okay but discouraged. Same as 20e+22
```
Special float values including `nan` and `inf` are allowed, as well as float value -0.0 is valid which should map according to IEEE 754. Unlike [TOML](https://github.com/toml-lang/toml), negative `nan` is dissalowed. Both `nan` and `inf` are discouraged as they remove some strict typing.
```toml
float = -0.0 # Okay
flt = inf # Okay, positive infinity
f = -inf # Okay, negative infinity
example = nan # Okay
n = -nan # ERROR
```
You can represent any float using the float type.
```toml
# fractional
float = 1.0<float> # Okay
flt = 5e+22<float> # Okay
f = 6.626e-34<float> # Okay
example = 6_000.1<float> # Okay
```
For even stricter typing, you can use generics.
```toml
float = 1.0<float<fraction>> # Okay
flt = 5e+22<float<exponent>> # Okay
f = 6.626e-34<float<exponent>> # Okay. Even with a fraction at the start it counts as an exponent
example = 6.1<float<exponent>> # ERROR
```
You can even do positive/negative typing like so:
```toml
float = 1.0<float<fraction<positive>>> # Okay
flt = 5e+22<float<exponent<positive>>> # Okay
f = 6.626e-34<float<exponent<negative>>> # Okay. Even with a fraction at the start it counts as an exponent
example = 6e-34<float<exponent<positive>>> # ERROR
# And so on
```
#### Boolean
Booleans are always true or false values.
```toml
boolean = true
bool = false
```
You can represent any boolean using the boolean type.
```toml
boolean = true<boolean> # Accurate but useless since it is interpreted
```
Since booleans are not very complex. Generics can not be used for stricter typing.
#### Offset Date-Time
Unlike [TOML](https://github.com/toml-lang/toml), to unambiguously represent a specific instant in time, you may use an [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html) formatted date-time with offset.
```toml
offdatetime = 1979-05-27T07:32:00Z
odt = 1979-05-27T00:32:00-07:00
o = 1979-05-27T00:32:00.999999-07:00
```
Millisecond precision is required. Further precision of fractional seconds is implementation-specific. If the value contains greater precision than the implementation can support, the additional precision must be truncated, not rounded. This goes for all time formats in TypeFile. ***(message from TypeFile dev - I have no clue what this means)***
You can represent any Offset Date-Time using the abbreviated odt type.
```toml
offdatetime = 1979-05-27T07:32:00Z<odt> # Okay
odt = 1979-05-27T00:32:00-07:00<odt> # Okay
o = 1979-05-27 00:32:00.999999-07:00<odt> # Okay
```
For even stricter typing, and in this case readability, you can use generics.
```toml
offdatetime = 1979-05-27T07:32:00Z<odt> # Okay
odt = 1979-05-27T00:32:00-07:00<odt<mt>> # Okay, represents MT or Mountain Time
o = 1979-05-27 00:32:00.999999-07:00<odt<pst>> # ERROR
```
#### Local Date-Time
If you remove the offset from a date-time, it will represent the given date-time without any relation to an offset or timezone. It cannot be converted to an instant in time without additional information. Conversion to an instant, if required, is implementation-specific.
```toml
localdatetime = 1979-05-27T07:32:00
ldt = 1979-05-27T00:32:00.999999
```
As mentioned before, millisecond precision is required. Further precision of fractional seconds is implementation-specific. If the value contains greater precision than the implementation can support, the additional precision must be truncated, not rounded. This goes for all time formats in TypeFile.
You represent any Local Date-Time using the abbreviated ldt type.
```toml
localdatetime = 1979-05-27T07:32:00<ldt> # Okay
ldt = 1979-05-27T00:32:00.999999<ldt> # Okay
# These are both useless since they would be interpreted
```
Since this type does not include timezones. Generics can not be used for stricter typing.
#### Local Date
If you include only the date portion of a date-time, it will represent that entire day without any relation to an offset or timezone.
```toml
localdate = 1979-05-27
ld = 2021-06-6
```
You can represent any Local Date using the abbreviated ld type.
```toml
localdate = 1979-05-27<ld> # Okay
ld = 2021-06-6<ld> # Okay
# These are both useless since they would be interpreted
```
Since this type does not include timezones. Generics can not be used for stricter typing.
#### Local Time
If you include only the time portion of a date-time, it will represent that time of day without any relation to a specific day or any offset or timezone.
```toml
localtime = 07:32:00
lt = 00:32:00.999999
```
You can represent any Local Date using the abbreviated lt type.
```toml
localtime = 07:32:00<lt> # Okay
lt = 00:32:00.999999<lt> # Okay
# These are both useless since they would be interpreted
```
Since this type does not include timezones. Generics can not be used for stricter typing.
#### Array
Arrays are square brackets with a list of values inside. Whitespace is ignored. Elements are separated by commas. Arrays can contain values of the same types as allowed in key/value pairs. Values of different types may be mixed.
```toml
integers = [ 1, 2, 3 ]
strings = [ "red", "yellow", 'green' ]
nested_arrays_of_ints = [ [ 1, 2 ], [3, 4, 5] ]
nested_mixed_array = [ [ 1, 2 ], ["a", "b", "c"] ]

# Mixed types in arrays are allowed
numbers = [ 0.1, 0.2, 0.5, 1, 2, 5 ]
strings_and_integers = [ 'hello', -1, "hi" ]
```
Arrays can span over multiple lines. Unlike [TOML](https://github.com/toml-lang/toml), A terminating comma (also called a trailing comma) after the last value of the array is allowed but discouraged. Any number of newlines and comments may precede values, commas, and the closing bracket. Indentation between array values and commas is treated as whitespace and ignored.
```toml
integers = [
  1, 2, 3
]

ints = [
  1,
  2, # Okay
]
```
You can represent an array by specifying the types inside the array followed by `[]`.
```toml
integers = [ 1, 2, 3 ]<integer>[] # Okay
strings = [ 'hello', "world" ]<string>[] # Okay
union = [ 3.1415, false ]<float|boolean>[] # Okay
example = [ 1, 'string' ]<integer>[] # ERROR
# The type declaration must touch the end of the array
multi = [
1,
'string', # Trailing comma allowed but discouraged
]
<string|number>[] # ERROR
# All examples that ended in "Okay" are usless since they would be interpreted, a more useful case would be:
unknown = [ 'string' ]<number|string>[] # Okay. Number may be added to array later
```
For even stricter typing, you can use generics.
```toml
integers = [ 1, -2, 3 ]<integer<decimal<positive|negative>>>[] # Okay
strings = [ 'hello', "world" ]<string<single|double>>[] # Okay
union = [
3.1415,
false
]<float<fraction>|boolean>[] # Okay
```
That's it for all the types. Here are some final examples to wrap things up.
```toml
integers.decimals = [ 1, 2, "3", '4' ]<string<single|double>|integer<decimal>>[]
unknown = null?
any_array = [ 'hello world', 1 ]!
empty = []<string>[]
error = 1<number>! # Cant be any type when type number is already declared
expo = 20e-11<float<exponent<negative>>>
```
### Tables
Tables (also known as hash tables or dictionaries) are collections of key/value pairs. They are defined by headers, with square brackets on a line by themselves and will always implicity be an Object type. You can tell headers apart from arrays because arrays are only ever values. Tables should always be typed in [Camel Case](https://en.wikipedia.org/wiki/Camel_case).
```toml
# Example table header
[table]
```
Under that, and until the next header or EOF, are the key/values of that table. Key/value pairs within tables are not guaranteed to be in any specific order. Tables are syntactictly similar to Keys.
```toml
[strings_and_decimals]
key = 'value'<string>
decimal = 2<integer<decimal>>

[owner]
name = "zahtec"
age = 100
cool = false
```
.. Continue tommorow. Add Object type to type list. ..
