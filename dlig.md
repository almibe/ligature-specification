# DLig the Dynamic Input Format for Lig

## Introduction

DLig is superset of Lig that is intended for use when manually inputting Statements into Ligature.
It does this by providing the following features:

 * Copy characters `^`, that simply copy the part (Entity, Attribute, or Value) of the Statement above it down depending on its location in the Statement.
 * Prefix shortening `prefix long = this:is:pretty:long:` which allows you to type long identifiers easier.
 * ID generation `{}`, which will create an automatically generated ID and works with prefixing.

## Copies

Using the copy character the following DLig

```
<a> <b> <c>
^ <d> ^
^^ <e>
```

would become the following in Lig

```
<a> <b> <c>
<a> <d> <c>
<a> <d> <e>
```

Using a copy character in the first Statement of a file will result in an error.

## Prefix Shortening

Ligature's approach to prefix shortening is similar to Turtle with some syntax changes, and currently there is no support for base.
At the top of a DLig file you can list a several prefixes.

```
prefix long = this:is:pretty:long:
prefix longer = this:is:actually:longer:
prefix very:short = a:

```

## ID Generation

Example:

`<this:id:is:generated:{}>`

Example with prefix shortening:

```
prefix this = this:id:is:generated:
this:{}
```
