# LIGATURE

## Introduction
Ligature is a project that

## Data Model
Ligature tries to have a very minimal data model.
Currently, Ligature only has a handful of data types that are required.

### Dataset
A dataset in Ligature is a named set of statements.

### Statement
A statement is tuple of an entity, an attribute, and a value.
A value can be either an entity or a literal.

### Entity
An entity is something we can make statements about.
An entity by itself is not interesting and we know nothing meaningful about an entity by itself.
We can only know things about entities based on the statements that exist that describe that entity.

### Attribute
An attribute is a named relationship between an entity and a value.

### Value
A value can be either a literal or an entity.

### Literals
Ligature supports four kinds of literals currently.

#### String Literal
A UTF-8 string.

#### Integer Literal
A 64-bit signed integer.

#### Float Literal
An IEEE-754 64-bit floating-point number.

#### Bytes Literal
An array of bytes.
