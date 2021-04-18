# LIGATURE

## Introduction
Ligature is an open source knowledge graph.

## Data Model
Ligature tries to have a very minimal data model.
Currently, Ligature only has a handful of data types that are supported.

### Dataset
A dataset in Ligature is a named set of statements.

### Statement
A statement is tuple of an entity, an attribute, and a value.
A value can be either an entity or a literal.

### PersistedStatement
When a Statement is saved into a knowledge graph a Persisted Statement is created.
A Persisted Statement is a tuple of a Statement and a new Entity called a Context.
This Context Entity uniquely identifies the given Statement and allows for making Statements about Statements.

### Entity
An entity is something we can make statements about.
An entity by itself is not interesting and we know nothing meaningful about an entity by itself.
We can only know things about entities based on the statements that exist that describe that entity.
An Entity is represented by an integer and that integer is used to reference Entities.

### Attribute
An attribute is a named relationship between an entity and a value.

### Value
A value can be either a literal or an entity.

### Literals
Ligature supports four kinds of literals currently.

#### String Literal
A UTF-8 encoded string.

#### Integer Literal
A 64-bit signed integer.

#### Float Literal
An IEEE-754 64-bit floating-point number.

#### Bytes Literal
An array of bytes.
