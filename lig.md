# The Lig Serialization Format

Lig is a text format for representing Ligature's data model.
It also has some features to help with manually adding data into Ligature.
It takes influence from formats like N-Triples and Turtle, but has a number of differences specific for Ligature.

## Identifiers

Identifier in Lig follow the same rules as Ligature proper see [ligature.md](ligature.md#identifiers).

```
<123e4567-e89b-12d3-a456-426614174000>
<1>
<hello>
<https://stuff.things>
<game:player:bob>
<height>
<person:age>
<http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
```

## Values

Values are represented in a number of ways depending on the type of the value.
 * Entities are represented the same way as above
 * Strings are wrapped in `"` and follows the definition of Strings from JSON
 * Integers are numbers that don't contain a decimal point (limited to 64 bits)
 * Byte vectors starts with `0x` and contains only hex characters 0-9 and a-f

```
"I'm a string."
1234567890
0xDEAD
```

## Statements

Statements written in Lig are full Ligature Statements.
They contain an Entity, an Attribute, and a Value on a single line.

```
<entity:a> <attribute:b> "Hey"
<entity:b> <attribute:b> 2345678
```

## Copies

Using the copy character the following DLig

```
<a> <b> <c>
^ <d> ^
^^ <e>
```

would become the following in Lig

```
<a> <b> <c>
<a> <d> <c>
<a> <d> <e>
```

Using a copy character in the first Statement of a file will result in an error.

## Prefix Shortening

Ligature's approach to prefix shortening is similar to Turtle with some syntax changes, and currently there is no support for base.
At the top of a DLig file you can list a several prefixes.
Then when you go to use a prefix you simply don't wrap the Entity in `<>`'s and separate the prefix from the
suffix with a `:`.
Prefix names must be valid Identifiers but they can't contain `:`.

```
prefix long = this:is:pretty:long:
prefix longer = this:is:actually:longer:
prefix short = a:

long:5 

```

## ID Generation

Example:

`<this:id:is:generated:{}>`

Example with prefix shortening:

```
prefix this = this:id:is:generated:
this:{}
```

## Syntax

Below is some pseudo-ebnf for lig.
At some point I need to make this a little more formal and follow ebnf more strictly.

```
ligDocument       ::= statement*
statement         ::= IDENTIFIER IDENTIFIER value
value             ::= IDENTIFIER | INTEGER_LITERAL | STRING_LITERAL | BYTES_LITERAL

IDENTIFIER      ::= <[a-zA-Z_][a-zA-Z0-9\-._~:/?#\[\]@!$&'()*+,;%=]*>
INTEGER_LITERAL ::= [0-9]+
STRING_LITERAL  ::= "(([^\x00-\x1F\"\\]|\\[\"\\/bfnrt]|\\u[0-9a-fA-F]{4})*)"
BYTES_LITERAL   ::= '0x' ([0-9a-f]{2})+
```
