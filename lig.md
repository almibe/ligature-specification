# The Lig Serialization Format

## Basics

### Identifiers

Identifier in Lig follow the same rules as Ligature proper see [ligature.md](ligature.md#identifiers).

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
 * Strings are wrapped in `"` and follows the definition of Strings from JSON
 * Integers are numbers that don't contain a decimal point (limited to 64 bits)
 * Floating point numbers are numbers that contain a decimal point (64 bit IEEE 754)
 * Byte vectors starts with `0x` and contains only hex characters 0-9 and a-f

### Statements

Statements written in Lig are full Ligature Statements.
So they contain an Entity, an Attribute, a Value, and a Context.
Contexts are just Entities that represent a Statement.
Note that Contexts must be unique for a given dataset.

## Syntax

Below is some pseudo-ebnf for lig.
At some point I need to make this a little more formal and follow ebnf more strictly.

```
ligDocument       ::= (completeStatement statement*)*
completeStatement ::= entity attribute value entity
statement         ::= ENTITY ATTRIBUTE value ENTITY
value             ::= ENTITY | FLOAT_LITERAL | INTEGER_LITERAL | STRING_LITERAL | BYTES_LITERAL

ENTITY          ::= '<' IDENTIFIER '>'
ATTRIBUTE       ::= '@<' IDENTIFIER '>'
IDENTIFIER      ::= [a-zA-Z_][a-zA-Z0-9\-._~:/?#\[\]@!$&'()*+,;%=]*
FLOAT_LITERAL   ::= [0-9]+ '.' [0-9]+
INTEGER_LITERAL ::= [0-9]+
STRING_LITERAL  ::= "(([^\x00-\x1F\"\\]|\\[\"\\/bfnrt]|\\u[0-9a-fA-F]{4})*)"
BYTES_LITERAL   ::= '0x' ([0-9a-f]{2})+
```
