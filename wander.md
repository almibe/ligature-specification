# Wander

Wander is a scripting language for working with knowledge graphs in Ligature.
While Wander's focus is on working with Ligature it can be used as a general scripting language as well.

## Status

Work on Wander is still in early days.
Expect changes and some differences between this document and implementations for the time being.

## Goals of Wander

 - be a small and easy to learn language for most people with any scripting background
 - make heavy use of streams (no manual loops), expressions, and pattern matching to solve problems
 - support immutability and functional concepts while not worrying about be purely functional
 - provide a variety of options for handling the output of a script (tables, json, csv, visualizations)
 - be easy to implement and understand

## Literal Types

 * Integer - a signed 64-bit integer (Java's long or Rust's i64)
  * 1
  * -20
 * String - a UTF-8 string, that follows the definition of a JSON string
  * "Hello"
  * "Hello\tWorld"
 * Byte Array - an array of bytes
  * 0xF0
  * 0x48EF120A59
 * Identifier - Identifiers are wrapped in back ticks, just like lig
  * \`hello\`
  * \`https://ligature.dev\`
 * Boolean
  * true
  * false
 * Lambdas
  * \x -> x
 * Arrays
  * [1, 2, "Hello"]
 * Records
  * { x = 5, y = "Hello" }

## Keywords

Ligature tries to have a minimal number of keywords.
Keywords are names that users cannot use to define bindings.
This list is likely to change but here is the current list of keywords.

 * true
 * false
 * nothing
 * when

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

## Arrays

Arrays are similar to lists or vectors in other languages.
They are an ordered list of items.

```wander
[1, 2, 3],              -- A Sequence of Integers
[`a`, `b`, `c`],        -- A Sequence of Identifiers
[`a` `b` `c`],          -- A Sequence of Statements, no commas between Identifiers
[],                     -- An empty Array
[[1], [], [45 -23]],    -- An Array of Arrays of Integers
```

## Records

Records are a data structure that holds key value pairs where the key is a string of characters defined by 

```regex
[a-zA-Z_][a-zA-Z0-9_]*
```

while the value can be any Wander value.
Each value in a record can be of a different type and this is represented in the type system.
The following code creates a record with 3 rows that contain Ints, a row called color that contains a byte array,
and a row named label that contains the string "You".

```wander
location = { x = 1, y = 5, z = 10, color = 0xFFFFFF, label = "You" }
-- location has the type Record(x::Int y::Int z::Int color::Bytes label::String)
```

## Comments

Comments in Wander are marked by `--` and continue until the end of a line.

`24601 -- what does this even mean?`

## Let Expressions

Let expressions allow a user to bind a value to a name within the current scope.

`prisonerNumber = 24601 -- ohh`

## Expressions

Everything in Wander can be viewed as an expression.
By expression I mean they result in a value.

## Scopes

In Wander a scope is wrapped in `()` and acts as an expression that is evaluated.
Scopes are also used for scoping variables.
See a small example below.

```wander
x = 5, -- x is now 5 at the top scope

y = (    -- create a new scope
  x = 6, -- in this scope x is 6
  x      -- return 6
),       -- end scope

y        -- this will return 6
```

A Ligature script is made up of several scopes.
The first two are created for the user and all others are created by the user.
Scope 0 contains "native functions" these are functions that the user didn't define.
Scope 1 contains the main script.
Scopes 2 and up are all defined by the user.

## Lambdas

Wander supports Lambdas.
Lambdas are treated like a normal value and can be assigned to a variable, passed to a function, or returned from a function.
A very basic example is:

```
five = \ _ -> 5,              -- assign a closure with no arguments to the name five
five (),                      -- call the closure, results in 5
double = \x -> mult 2 x,  -- define a closure with a single argument
ten = double (five ()),
middle = \first second third -> second -- a closure with three arguments
```

## A Note on Functions

Wander doesn't currently support "normal" function declarations.
Right now only Lambdas are supported.
I'm currently going to see how far only supporting Lambdas get us, and if they are too limited functions will be added.
The main thing that functions will add will be overloading, but there are also other ways to potentially handle that.

## When Expressions

TODO

## Pipe Operator

The pipe operator is expressed as `|` and it allows the value on the left to be passed to the function on the right as the last value.

```
(or false (not(not(true))))  -- true
true | not | not | or false  -- true
```

## Standard Library

### Boolean Functions

| Name | Signature         | Example         | Result |
| ---- | ----------------- | --------------- | ------ |
| not  | bool -> bool      | not(true)       | false  |
| and  | bool bool -> bool | and(true false) | false  |
| or   | bool bool -> bool | or(true false)  | true   |

### Integer Functions

| Name | Signature         | Example         | Result |
| ---- | ----------------- | --------------- | ------ |
| add  | int int -> int    | add(1 2)        | 3      |
| sub  | int int -> int    | sub(1 2)        | 3      |
| mul  | int int -> int    | mul(1 2)        | 3      |
| div  | int int -> int    | div(1 2)        | 3      |
| gt   | int int -> bool   | gt(1 2)         | false  |
| lt   | int int -> bool   | lt(1 2)         | true   |

### String Functions

eq

cat

substring

matches

beginsWith

endsWith

### Sequence Functions

#### at

#### each

Performs a give task on each element in a Seq.

```wander
each([1 2 3] { x -> log(x) })
```

#### filter

