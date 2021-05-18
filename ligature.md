# LIGATURE

## Introduction
Ligature is an open source knowledge graph.

## Data Model
Ligature tries to have a very minimal data model.
Currently, Ligature only has a handful of data types that are supported.

### Dataset
A Dataset in Ligature is a named set of Statements.

### Statement
A Statement is tuple of an Entity, an Attribute, a Value, and a Context.
A Value can be either an Entity or a Literal.
A Context is an Entity that uniquely identifies a Statement within a Dataset.

### Identifiers

The definition of a valid Identifier is used for both Entity identifiers and Attribute names.
Currently, a valid Identifier is a string that starts with either an underscore or a character from a-z or A-Z,
and is followed any number of characters that are valid in URLs.
This will probably be revisited at some point but initially I think this will work well for most uses.
Below is the regular expression that expresses what a valid Identifier is.

```regexp
[a-zA-Z_][a-zA-Z0-9-._~:/?#\[\]@!$&'()*+,;%=]*
```

### Entity
An Entity is something we can make Statements about.
An Entity by itself is not very interesting,
and we know nothing meaningful about an Entity by itself except for its identifier.
We can only know things about Entities based on the Statements that exist that describe that Entity.

### Attribute
An Attribute is a named relationship between an Entity and a Value.

### Value
A Value can be either a Literal or an Entity.

### Literals
Ligature supports four kinds of Literals currently.

#### String Literal
A UTF-8 encoded string.

#### Integer Literal
A 64-bit signed integer.

#### Float Literal
An IEEE-754 64-bit floating-point number.

#### Bytes Literal
An array of bytes.

### Context
The last part of a Statement is the Context.
The Context is just an Entity that uniquely represents a Statement within a Dataset.
In practice a Context will not have a meaningful name and will usually consist of a namespaced UUID.
For example, `my:dataset:66b42eac-c894-4c7b-af3a-aff367a6ecb9`.
