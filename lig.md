# The Lig Serialization Format

Lig is a text format for representing Ligature's data model.
It uses types from Wander.
The format of the file is line based and is as follows.

```
FILE = DATASET*

DATASET = DATASETNAME
          STATEMENTS

DATASETNAME = string
STATEMENTS = statement*
```
