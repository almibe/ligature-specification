# Ligature REPL

Ligature comes with a REPL that combines many of the features of Ligature.
Namely Lig, DLig, Wander, different stores, and HTTP.
This project is still under design so expect changes/incomplete implementations.
Ligature's REPL is based on issuing commands.
Commands all start with a colon.
A command can either cause a single action to take place or it can switch the REPL to a different mode.

Some examples of an action command would be:

`:create-dataset HelloData`
`:datasets`
`:delete-dataset HelloData`

Some examples of a command that changes mode:

`:wander`
`:dlig`

## Action Commands

### `:instance`

Reports the current instance the REPL is using.
Currently the REPL can only work with a single instance of Ligature.
Example output could be the following

* `in-memory`
* `h2:pathToDatabaseFile`
* `http://localhost:4567`

### `:use {instance}`

Switches the current instance.
The value of `{instance}` would match the output of the `:instance command`.

### `:datasets`

Prints all of the Datasets in the current instance.

### `:add-dataset {datasetName}`

Adds a Dataset to the current instance.

### `:remove-dataset {datasetName}`

Removes Dataset from the current instance.

### `:rename-dataset {oldName} {newName}`

Renames a Dataset in the current instance.
This command will not overwrite an existing Dataset.

### `:copy-dataset {from} {to}`

Makes a copy of a Dataset.
This command will not overwrite an existing Dataset.

### `:merge-datasets {from} {to}`

Adds all of the contents of the `from` Dataset to the `to` Dataset.
This command will not create a new Dataset if `to` doesn't exist.

### `:exit`

Exits the REPL.

## Mode Commands

### `:insert {datasetName}` -> `:commit` | `:cancel`

Allows you to enter Statements in DLig to a given Dataset and then either commit or cancel the transaction.

Example

```
>:insert testDataset
dlig><this> <is> <aStatement>
dlig>:commit
>
```

### `:remove {datasetName}` -> `:commit` | `:cancel`

Allows you to remove Statements via DLig to a given Dataset and then either commit or cancel the transaction.

Example

```
>:remove testDataset
dlig><this> <is> <aStatement>
dlig>:commit
>
```

### `:wander {datasetName}` -> `:run` | `:cancel`

Allows you to run a Wander script against a given Dataset.

Example

```
>:wander testDataset
wander><this> <is> <aStatement>
wander>:run
<this> <is> <aStatement>
>
```
