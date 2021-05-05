# The Lig Serialization Format

## Basics

### Identifiers

The definition of a valid Identifier is used for both Entity identifiers and Attribute names.
Currently, a valid Identifier is a string that starts with either an underscore or a character from a-z or A-Z,
and is followed any number of characters that are valid in URLs.
This will probably be revisited at some point but initially I think this will work well for most uses.
Below is the regular expression that expresses what a valid Identifer is.

```regexp
[a-zA-Z_][a-zA-Z0-9-._~:/?#\[\]@!$&'()*+,;%=]*
```

### Entities

Entities are represented by a valid Identifier that is wrapped in `<` and `>`

```
<hello>
<https://stuff.things>
<game:player:bob>
```

### Attributes

Attributes are represented similarly to Entities but are prefixed with `@`

```
@<height>
@<person:age>
@<http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
```

### Values

Values are represented in a number of ways depending on the type of the value.
 * Entities are represented the same way as above
 * Strings are wrapped in `"` and can contain new lines
 * Integers are numbers that don't contain a decimal point (limited to 64 bits)
 * Floating point numbers are numbers that contain a decimal point (64 bit IEEE 754)
 * Byte vectors starts with `0x` and contains only hex characters 0-9 and a-f

### Statements

Statements written in Lig are full Ligature Statements.
So they contain an Entity, an Attribute, a Value, and a Context.
Contexts are just Entities that represent a Statement.

### Wildcards

Wildcards are represented by `_` and mean to use the value in the position above in this position for this Statement

## Example

```
<thisIsAnEntity> @<thisIsAnAttribute> <thisIsAnEntityAsAValue> <context1>
_ @<thisIsAnotherAttribute> 23413123 <context2>
_ _ _ <context3>
```

Is the same as:

```
<thisIsAnEntity> @<thisIsAnAttribute> <thisIsAnEntityAsAValue> <context1>
<thisIsAnEntity> @<thisIsAnotherAttribute> 23413123 <context2>
<thisIsAnEntity> @<thisIsAnotherAttribute> 23413123 <context3>
```
