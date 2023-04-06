# Wander

Wander is a scripting language for working with knowledge graphs in Ligature.
Wander tries to combine ideas from modern general purpose languages (mainly Rust, Scala, and Koka)
while focusing just on what's needed for working with knowledge graphs instead of being a general purpose language.

## Status

Work on Wander is still in early days.
Expect changes and some differences between this document and implementations for the time being.

## Goals of Wander
 - be a small and easy to learn language for most people with any scripting background and some interest in knowledge graphs
 - make heavy use of streams (no manual loops), expressions, and pattern matching to solve problems
 - support all features SPARQL that make sense outside of the realm of RDF
 - support immutability and functional concepts while not worrying about be purely functional
 - provide a variety of options for handling the output of a script (tables, json, csv, visualizations)
 - be easy to implement and understand

## Types

Ligature is different from many other languages because it supports only a set number of types.
User defined types aren't supported since the main goal of Wander is to work with Ligature's knowledge graphs.
For the most part types are borrowed from Ligature and only a couple of new types are added.

### Types from Ligature

 * Integer - a signed 64-bit integer (Java's long or Rust's i64)
  * 1
  * -20
 * String - a UTF-8 string, that follows the definition of a JSON string
  * "Hello"
  * "Hello\tWorld"
 * Byte Array - an array of bytes
  * 0xff
  * 0x12
  * 0x48ef120a59
 * Identifier - Identifiers are wrapped in angle braces, just like lig
  * <hello>
  * <https://ligature.dev>

### Wander types not in Ligature

 * Boolean
 * Lambdas
 * Sequences/Streams
 * Graphs - an in-memory Dataset that acts like a mutable, non-transactional Dataset

## Keywords

Ligature tries to have a minimal number of keywords.
Keywords are names that users cannot use to define bindings.
This list is likely to change but here is the current list of keywords.

 * let
 * if
 * elsif
 * else
 * true
 * false
 * nothing

## Names

Names in Wander are used for variable names in scripts.
Variable names in Wander are used for local variables, top level variables, parameters, and function names.
A valid identifier starts with a-z, A-Z, or _ and then includes zero of more characters from the same set or numbers.

```regex
[a-zA-Z_][a-zA-Z0-9_]*
```

## Results

The main purpose of Wander is to produce a value as the result of a script.
This result can be as simple as a single value, but more likely it will involve building a table or graph of result values.
Wander uses it's internal Graph type to represent both table and graph results in the UI.

## Tuples

Tuples are an experimental feature in Wander.

```wander
(1 2 3)
(<a> <b> <c>)
()
```

## Sequences

Sequences are an experimental feature in Wander.

```wander
[1 2 3]
[<a> <b> <c>]
[]
```

## Graphs

Graphs are an experimental feature in Wander.

Create a graph by calling the `graph()` function.
See the list of functions that work with graphs below.

## Maps

Maps are an experimental feature in Wander.

```wander
["key": "value" <key>: 56]
```

## Comments

Comments in Wander are marked by `--` and continue until the end of a line.

`let x = 5 -- bind the value 5 to x`

## Let Statements

Let statements allow a user to bind a value to a name within the current scope.

`let x = 5`

## Expressions

Everything other than let statements in Wander can be viewed as an expression.
By expression I mean they result in a value.

## Scopes

In Wander a scope is wrapped in `{}` and acts as an expression that is evaluated.
Scopes are also used for scoping variables.
See a small example below.

```
let x = 5 -- x is now 5 at the top scope

let y = {   -- create a new scope
  let x = 6 -- in this scope x is 6
  x         -- return 6
}           -- end scope

y           -- this will return 6
```

A Ligature script is made up of several scopes.
The first two are created for the user and all others are created by the user.
Scope 0 contains "native functions" these are functions that the user didn't define.
Scope 1 contains the main script.
Scopes 2 and up are all defined by the user.

## Closures

Wander supports closures.
Closures are treated like a normal value and can be assigned to a variable, passed to a function, or returned from a function.
A very basic example is:

```
let five = { -> 5 } -- assign a closure with no arguments to the name five
five()              -- call the closure, results in 5
let double = { x -> mult(x x) } -- define a closure with a single argument
let ten = double(five())
let immediatelyFive = { -> 5 }() -- define and immediately call a closure

immediatelyFive()  -- an error since immediatelyFive is the value 5, and not a closure

let middle = { first second third -> second } -- a closure with three arguments
```

## A Note on Functions

Wander doesn't currently support "normal" function declarations.
Right now only closures are supported.
I'm currently going to see how far only supporting closures get us, and if they are too limited functions will be added.
The main thing that functions will add will be overloading, but there are also other ways to potentially handle that.

## A Note on Operators

Currently, Wander doesn't really support operators.
It's basically just the = used in let statements.
This might change but for now I'm trying to avoid them.
This means that everywhere in a normal C-style language you'd use an operator in Wander you call a function.

`1 + 2`

`add(1 2)`

or

`1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9`

`add(1 2 3 4 5 6 7 8 9)`

But the idea is that you normally won't do this but pass functions to other functions.


## Conditionals

### If Expressions

If expressions represent a choice between various blocks of code to run.
In Wander all if expressions are required to have both an `if` and `else` branch.
Optionally zero or more `elsif` branches can exist between them.
`if` and `elsif` cases accept a condition expression that must result in a boolean value.

```wander
let five = if true 5 else 6
if eq(five 5) 5 elsif eq(five 6) 6 else 7
if eq(five 5) {
  5
} elsif eq(five 6) {
  6
} else {
  let result = 7
  result
}
```

### Match Expressions

Match Expressions are being considered.

## Closure Calling Syntax, A Little Razzel-Dazzel

Wander tries to be a pretty simple and regular language.
One piece of syntax sugar in the language is lifted from languages like Koka and Nim
is the ability to call closures in one of two ways.

Function syntax

```wander
hello()
world(2)
multipleArgs(1 2 3 "Sup" <identifier> false)
```

Method syntax

```
-- closures with zero arguments can't use method syntax...
hello()

-- the first argument can be on the left of the closure name
-- followed by a period, the closure name, and the remainder of the arguments
2.world()
1.multipleArgs(2 3 "Sup" <identifier> false)

-- closures can also be included inline in a method call
-- the below line would result in "hello"
"hello".{ first second -> first }("world")
```

## Standard Library

### Boolean Functions

| Name | Example              | Result |
| ---- | -------- ----------- | ------ |
| not  | not(true)            | false  |
| and  | and(true false true) | false  |
| or   | or(true false true)  | true   |

### Integer Functions

add

sub

mul

div

gt

lt

### Floating Point Functions

...

### String Functions

eq

cat

substring

matches

beginsWith

endsWith

### Sequence Functions

#### each

Performs a give task on each element in a Seq.

```wander
each([1 2 3] { x -> log(x) })
```

#### filter

