# Wander

Wander is an experimental scripting language for working with knowledge graphs.
It is currently being developed as part of the Ligature project.
Wander tries to combine ideas from modern general purpose languages (mainly Kotlin, Scala, and Rust)
while focusing just on what's needed for working with knowledge graphs.

## Introduction

### Goals of Wander
- be a small and easy to learn language for most people with any scripting background and some (knowledge of|interest in) linked data
- make heavy use of streams (no manual loops), expressions, and pattern matching to solve problems
- support all features SPARQL that make sense outside of the realm of RDF
- support immutability and functional concepts
- provide a variety of options for handling the output of a script (tables, json, csv, visualizations)

In Wander everything is a function call, a reference to a symbol, or a literal except for the when operator.
Supplied functions and symbol references appear as plain text while user defined functions and references are prefixed with `$`.

### Method syntax
When you use a period in Wander you invoke method calling.
This means that the first argument to the function you are calling is the result of the last expression.

example of a problem that is hard to solve in sparql that should be easier to solve in Wander
- https://web.archive.org/web/20150516154515/http://answers.semanticweb.com:80/questions/9842/how-to-limit-sparql-solution-group-size

built in functions
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

### Example code

```
bind($datasetName "dataset1")
bind($hardCodedEntity #23) ; you probably shouldn't do this much
bind($attributeName "attribute")
bind($stringLiteral "This is a string")
bind($bytesLiteral 0x45DEAD45)
bind($integerLiteral 2342234)
bind($floatLiteral 0.234)
bind($statement statement($hardCodedEntity $attributeName $bytesLiteral))
bind($list list())  ; creates an empty immutable list
bind($map map())
bind($set set())
bind($mutList mutList())  ; not sure if I need this
bind($mutMap mutMap())  ; not sure if I need this
bind($mutSet mutSet())  ; not sure if I need this
bind($statement $.match($hardCodedEntity $attribute "This value"))
bind($statements $.match(iri ? ? ?)) ; all statements with that subject

bind($lambda {$x $y -> +($x $y)}) # define a lambda

bind($nine $lambda(4, 5)) ; call it

bind($result = when { ; TODO figure out how I want to handle when
  datasets.count > 10 -> :morethanten
  datasets.count > 1 -> :singledigitplural
  else -> :oneorzero
}
```
