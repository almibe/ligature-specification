# LIGATURE

## Introduction
Ligature is an open source knowledge graph.

## Data Model
Ligature tries to have a very minimal data model.
Currently, Ligature only has a handful of data types that are supported.

```
Dataset { dataset_name: DatasetName, statements: Statement* }
DatasetName { name: String }
Identifier { id: String }
Value {
    Identifier { value: Identifier } |
    StringLiteral { value: String } |
    IntegerLiteral { value: i64 } |
    ByteArray { value: [u8] }
}
Statement { entity: Identifier, attribute: Identifier, value: Value }
```

### Datasets
A dataset in Ligature is a named collection of statements.
Valid dataset names are currently groups of characters that include `_ a-z A-Z 0-9` that can't start with a number and that are separated by single `/`.
This naming convention is likely to change.
Even though dataset names might seem like they nest (`test/test` looks like it is under `test`) this isn't the case.
A dataset is its own unique entity and stands alone from all other datasets.
Also, datasets are very different from named graphs in RDF.
For example with named graphs blank nodes are shared across graphs in a dataset, but in datasets blank nodes are unique to their dataset.

### Statement
A Statement is tuple of an Entity, an Attribute, and a Value.
A Value can be either an Entity or a Literal.

### Identifiers

The definition of a valid Identifier is used for both Entity identifiers and Attribute names.
Currently, a valid Identifier is a string that starts with either an underscore or a character from a-z or A-Z,
and is followed any number of characters that are valid in URLs.
This will probably be revisited at some point but initially I think this will work well for most uses.
Below is the regular expression that expresses what a valid Identifier is.

```regexp
[a-zA-Z0-9-._~:/?#\[\]@!$&'()*+,;%=]*
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
Literals in Ligature represent an immutable value.
Several types are currently supported with the possibility to add more.

#### String Literal
A UTF-8 encoded string.

#### Integer Literal
A 64-bit signed integer.

#### Bytes Literal
An array of bytes.
