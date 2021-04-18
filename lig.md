# The Lig Serialization Format

## Basics

Entities are represented wrapped in `<` and `>`

```
<hello>
<https://stuff.things>
<game:player:bob>
```

Attributes are represented similarly to Entities but are prefixed with `@`

```
@<height>
@<person:age>
@<http://www.w3.org/1999/02/22-rdf-syntax-ns#type>
```

Values are represented in a number of ways depending on the type of the value.
 * Entities are represented the same way as above
 * Strings are wrapped in `"` and can contain new lines
 * Integers are numbers that don't contain a decimal point (limited to 64 bits)
 * Floating point numbers are numbers that contain a decimal point (64 bit IEEE 754)
 * Byte vectors starts with `0x` and may contain new lines

Comments start with `#` and continue until the end of the line

Wildcards are represented by `_` and mean to use the value in the position above in this position for this Statement

## Examples

```
# this is a comment
<thisIsAnEntity> @<thisIsAnAttribute> <thisIsAnEntityAsAValue>
_ @<thisIsAnotherAttribute> 23413123 # the underscore means use the above value in this position, the value in this case is an Integer
```
