# DLig the Dynamic Input Format for Lig

## Introduction

DLig is superset of Lig that is intended for use when inputting Statements into Ligature.
It does this by providing the following features:

 * Copies `.`, that simply copy the part (Entity, Attribute, or Value) of the Statement above it down depending on its location.
 * Prefix shortening `long = this:is:pretty:long` which allows you to 
 * ID generation `{}`, which will create an automatically generated ID and works with prefexing.

### Copies

Using the copy character the following DLig

```
<a> <b> <c> <d>
. <e> . <f>
.. <g> <h>
```

would become the following in Lig

```
<a> <b> <c> <d>
<a> <e> <c> <f>
<a> <e> <g> <h>
```

Using a copy character in the first Statement of a file will result in an error.
Also using a copy character in the Context position will result in an error.

### Prefix Shortening

Ligature's approach to prefix shortening comes from Turtle.
At the top of a DLig file you can list a single base and several prefixes.

```
@base 
@prefix long: this:is:pretty:long
@prefix longer: this:is:actually:longer
@prefix very:short: a


```

### ID Generation

