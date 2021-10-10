# Wander

Wander is an experimental scripting language for working with knowledge graphs in Ligature.
Wander tries to combine ideas from modern general purpose languages (mainly Kotlin, Scala, and Rust)
while focusing just on what's needed for working with knowledge graphs instead of being a general purpose language.

## Goals of Wander
 - be a small and easy to learn language for most people with any scripting background and some interest in knowledge graphs
 - make heavy use of streams (no manual loops), expressions, and pattern matching to solve problems
 - support all features SPARQL that make sense outside of the realm of RDF
 - support immutability and functional concepts
 - provide a variety of options for handling the output of a script (tables, json, csv, visualizations)

## Types

Ligature is different from many other languages because it supports only a set number of types.
User defined types aren't supported since the main goal of Wander is to work with Ligature's knowledge graphs.
For the most part types are borrowed from Ligature and only a couple of new types are added.

### Types from Ligature

 * Integer - a signed 64-bit integer
 * String - a UTF-8 string
 * Byte Array - an array of bytes
 * Identifier
 * Statement

### Wander types not in Ligature

 * Float - a 64-bit IEEE-754 floating point number
 * Boolean
 * Function

## Names

Names in Wander are used for variable names in scripts.
Variable names in Wander are used for local variables, top level variables, parameters, and function names.
A valid identifier starts with a-z, A-Z, or _ and then includes zero of more characters from the same set or numbers.

## Comments

Comment in Wander use the # symbol and comments run to the end of the current line.

`let x = 5 # bind the value 5 to x`

## Let Statements

Let statements allow a user to bind a value to a name within the current scope.

`let x = 5`

## Expressions

Everything other than let statements in Wander can be viewed as an expression.
By this I mean they result in a value.

## Scopes

In Wander a scope is wrapped in `{}` and acts as an expression.
Scopes are also used for scoping variables.
See a small example below.

```
let x = 5 # x is now 5 at the top scope

let y = { # create a new scope
  let x = 6 # in this scope x is 6
  x # return 6
} # end scope

y # this will return 6

```

## Operators

Currently, Wander doesn't really support many operators.
It's basically just the = used in let statements.
This might change but for now I'm trying to avoid them.

### Boolean Functions

| Name | Example                | Result |
| ---- | ---------------------- | ------ |
| not  | not(true)              | false  |
| and  | and(true, false, true) | false  |
| or   | or(true, false, true)  | true   |

### Integer Functions

### Floating Point Functions

### String Functions

## Conditionals

### If Expressions

### Match Expressions

## Functions

## Standard Library



<hr>

Note: everything below this line is pretty old and outdated and needs to be viewed/updated/deleted/merged into
the above document.

<hr>

### Relation to Scala/Kotlin/Rust/Modern Languages In General
 - use `let` to define immutable variables (mutable variables are not supported)
 - no types needed for declarations, but function params and returns need types
 - functions declared via lambdas
 - if and when expressions for control flow
 - denote ranges with `..`
 - support for `in` and `!in` for working with ranges and collections
 - support for `is` and `!is` for checking types

### Unique-ish concepts
 - In-memory graphs
  - a new data structure that represents a graph in-memory
  - create with graph() - all graph instances are mutable
 - Runs in read or write only modes

example of a problem that is hard to solve in sparql that should be easier to solve in Wander
 - https://web.archive.org/web/20150516154515/http://answers.semanticweb.com:80/questions/9842/how-to-limit-sparql-solution-group-size

built-in functions
 - collection/stream functions
 - SPARQL's functions that make sense (need to make a list)
 - `store.dataset(name)` - get a single dataset -- TODO probably delete this?
 - `store.datasets()` - get all datasets
 - `store.datasets(prefix)` - get datasets that start with prefix
 - `store.datasets(start, stop)` - get dataset in range
 - `store.createDataset(name)` - create a dataset
 - `store.deleteDataset(name)` - delete a dataset
 - `store.query(datasetname) { **query code** }` - query a dataset w

By default, scripts run in a read block, if you want to manipulate the store you must opt into a write block.

### Data Types

Wander comes with a set of predefined data types for working with Ligature.
Currently, user defined types are not allowed (eventually this will be revisited).

 * Integer - same as Ligature's type
 * Floating Point - same as Ligature's type
 * String - same as Ligature's type
 * Bytes - same as Ligature's type
 * Boolean - true/false (not found in Ligature itself)
 * Entity - same as Ligature's type
 * Attribute - same as Ligature's type
 * Value - same as Ligature's type (Either an Entity, Integer, Floating Point, String, or Bytes)
 * Option<T> - An optional value of a generic type, is either Some(T) or None
 * List<T> - an ordered, indexable, immutable list of items of type T
 * Set<T> - an unordered unique set of values of type T
 * Pair<A,B> - A single heterogeneous pair of two values.
 * Map<K,V> - A set of pairs.
 * Stream<T> - a single use stream of values.

### When Expressions

A `when` expression is the sole control flow mechanism in Wander.
It is based on the `when` expression from Kotlin.

### Functions

Functions in Wander are just lambdas that are stored in variables.
To define a function use the following syntax.
This is likely to change.

```wander
let add2Numbers = (x: Integer, y: Integer -> Integer) { x + y }
```

### Example code

```
let x = "hello"
let datasetName = dataset1 //could also be an attribute name
let hardCodedEntity = <_23> //you probably shouldn't do this much
let attributeName = @<attribute> //could also be a dataset name
let stringLiteral = "This is a string"
let bytesLiteral = 0x45DEAD45
let integerLiteral = 2342234
let floatLiteral = 0.234
let statement = hardCodedEntity attributeName bytesLiteral <_345875987457893>
let list = list()
let map = map()
let set = set()
let mutList = mutList()  ; not sure if I need this
let mutMap = mutMap()  ; not sure if I need this
let mutSet = mutSet()  ; not sure if I need this
let pair = 5 to "hello"  ; not sure if I need this
let statement = $.matchStatements(iri <http://predicate/something> :prefix)
;or
let specificStatements = $.match(iri <http://predicate/something> :prefix) //all statements that match pattern across all datasets
let statements = $.match(iri ? ? ?) #all statements with that subject

let lambda = {x,y -> x+y} #define a lambda

let nine = lambda(4, 5) #call it

let result = when {
  datasets.count > 10 -> :morethanten
  datasets.count > 1 -> :singledigitplural
  else -> :oneorzero
}

let useResult = result a :quantity

let result2 = when(useResult.subject) {
  :morethanten -> 11
  :singledigitplural -> 5
  else -> 0
}

let useResult2 = result2 * 100

let blah = graph()
let blah2 = map("hello" to "world")

blah.addStatements(list(
  useResult,
  :first test:second <#third> :graph2
))

when {
  blah.subjects.count > 90 -> return blah
  else -> return blah2
}
```

Below is some old text from the https://github.com/almibe/wander project that needs reviewed/merged into the above text.
Most of it is problem outdated.

In Wander everything is a function call, a reference to a symbol, or a literal except for the when operator.
Supplied functions and symbol references appear as plain text while user defined functions and references are prefixed with `$`.

### Method syntax
When you use a period in Wander you invoke method calling.
This means that the first argument to the function you are calling is the result of the last expression.

example of a problem that is hard to solve in sparql that should be easier to solve in Wander
- https://web.archive.org/web/20150516154515/http://answers.semanticweb.com:80/questions/9842/how-to-limit-sparql-solution-group-size

built-in functions
- collection/stream functions
- SPARQL's functions that make sense (need to make a list)

`$` represents the dataset a script is being ran against.
When running against the entire store

Methods on store (this will probably change since I'm not sure I want methods)
* `store.dataset(name)` - get a single dataset -- TODO probably delete this?
* `store.datasets()` - get all datasets
* `store.datasets(prefix)` - get datasets that start with prefix
* `store.datasets(start, stop)` - get dataset in range
* `store.createDataset(name)` - create a dataset
* `store.deleteDataset(name)` - delete a dataset
* `store.query(datasetname) { **query code** }` - query a dataset w
  By default, scripts run in a read block, if you want to manipulate the store you must opt into a write block.
