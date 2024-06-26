# The Ligature Serialization Format

This document assumes you are familiar with Ligature's model.

Ligature has a simple text format for writing out its data model.
It is different than some other text formats like XML or JSON because it is interpreted like a script.
A Ligature file is read and interpretted from top to bottom and the final result is what is read from the file.
This might sound a little confusing, but this is done to make entering data easier.
The [Dhall configuration language](https://dhall-lang.org/) has a similar approach with a very different use case.

## Writing Literals

Most literals in Ligature are written out how they would be in similar formats.
Strings follow the rules of JSON.
Numbers are written out like Integers in most programming languages.
Identifiers are wrapped in back ticks.

```ligature
1
"Hello,\nworld!"
`https://ligature.dev`
```

Bytes don't have a literal, I'll talk about them later in the section marked `Function Calls`.

## Writing Networks

Network literals define a set of Statements that are treated as a collection.

```ligature
{
  `a` `b` `c`,
  `b` `c` `d`
}
```

TODO: mention Value lists and Entity expansions.

## Comments

Comments in Ligature are marked by `--` and continue until the end of a line.

```ligature
24601 -- what does this even mean?
```

## Variable Binding

To bind a value to a name use the `=` to set a value.

```ligature
valjean = 24601 -- ohhh
```

## Identifier Concatenation

One of the benefits of having variables is being able to reuse parts of an identifier.

```ligature
base = `https://github.com/almibe/ligature-specification/`,
file = `README.md`,
-- all of the lines below result in the same Identifier
base:file,
base:"README.md",
base:`README.md`,
```

## Scopes

In Wander a scope is wrapped in `()` and acts as a single expression that is evaluated to a single value.
Scopes are also used for scoping variables.
See a small example below.

```ligature
x = 5,   -- x is now 5 at the top scope

y = (    -- create a new scope
  x = 6, -- in this scope x is 6
  x      -- return 6
),       -- end scope

y        -- this will return 6
```

## Function Calls

