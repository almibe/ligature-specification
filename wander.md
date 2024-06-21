# Wander

Wander is a scripting language for working with knowledge graphs in Ligature.
It takes inspiration from several programming paradigms including functional programming,
logic programming, and declarative programming.
While Wander's focus is on working with Ligature it can be used as a general scripting language as well.

## Status

Work on Wander is still in early days.
Expect changes and some differences between this document and implementations for the time being.

## Goals of Wander

 - be a small (no keywords, no user defined types or functions*) and easy to learn language
 - make heavy use of iterators (no manual loops), expression piping, and pattern matching to solve problems
 - be easy to implement and also provide tooling for

* Note: You can't define functions in Wander, but you can define functions in a host language and expose them to Wander

## Literal Types

 * Int - a signed, unbound Integer value, similar to a BigInt in most programming langauge
  * 0
  * -20
 * String - a UTF-8 string, that follows the encoding definition of a JSON string
  * "Hello"
  * "Hello\tWorld\n"
 * Identifier - Identifiers are wrapped in back ticks, just like in Ligature
  * \`hello\`
  * \`https://ligature.dev\`
 * Arrays
  * [1, 2, "Hello"]
 * Records
  * { x = 5, y = "Hello" }
 * Networks
  * { \`a\` \`b\` \`c\`, \`five\` \`=\` 5 }

### Networks

Network literals define a set of Statements that are treated as a collection.

```wander
{
  `a` `b` `c`,
  `b` `c` `d`
}
```

## Names

Names in Wander are used for variable names in scripts.
Variable names in Wander are used for local variables, top level variables, parameters, and function names.
A valid identifier starts with a-z, A-Z, or _ and then includes zero of more characters from the same set or numbers.

```regex
[a-zA-Z_][a-zA-Z0-9_]*
```

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

## Pipe Operator

The pipe operator is expressed as `|` and it allows the value on the left to be passed to the function on the right as the last value.

```
(or false (not(not(true))))  -- true
true | not | not | or false  -- true
```
