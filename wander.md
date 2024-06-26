# Wander

This document assumes you are familiar with Ligature's data model and text format.

Wander is a scripting language for working with Ligature's Semantic Networks.
It takes inspiration from several programming paradigms including functional programming,
logic programming, and declarative programming.
While Wander's focus is on working with Ligature it can be used as a general purpose scripting language as well.

## Status

Work on Wander is still in early days.
Expect changes and some differences between this document and implementations for the time being.

## Goals of Wander

 - be a small (no keywords, no user defined types or functions*) and easy to learn language
 - make heavy use of iterators (no manual loops), expression piping, and pattern matching to solve problems
 - be easy to implement and also provide tooling for

* Note: You can't define functions in Wander, but you can define functions in a host language and expose them to Wander.

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

Records are a data structure that holds key-value pairs where the key is an Identifier and the value is an Identifer, Int, Bool, or String.

```wander
{ `digit` = 5, `text` = "five" }
```

## Expressions

Everything in Wander can be viewed as an expression.
By expression I mean they result in a value.

## Call Chaining/Uniform Function Call Syntax

Often you want to use the result of one expression inside of another expression.

```
(or false (not(not(true)))) -- true
```

Some people (myself included) find reading code like this difficult.
To help with this languages like Wander provide an alternative syntax for calling functions
that encourages thinking about a pipeline of transformations.
In Wander this is done with the `.` operator.

Below are two examples of calling a function normally and then using the `.` operator.

```wander
-- examples with single argument function
not true, -- false
true.not, -- false

-- examples with multiple argument function
or false true, -- true
false.or true, -- true
```

This can be expanded to change calls.
Below is the same expression written on one line and broken up into multiple lines.

```
true.not.not.or false, -- true

true
  .not
  .not
  .or false,           -- true

```

I personally find the last example easier to read and edit than the initial example I started this section with.
