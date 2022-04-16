# Ligature HTTP API
Although implementations may provide embeddable libraries,
the main way Ligature is intended to be interacted with is via HTTP.

### Create Dataset
POST the following.

`/datasets/datasetName`

Returns:

If invalid Dataset name...

If Dataset exists...

If creation failed...

If creation succeeded...

### Delete Dataset
DELETE the following.

`/datasets/datasetName`

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If deletion failed...

If deletion succeeded...

### Add Statements to Dataset
POST the following.

`/datasets/datasetName`

With the following body and a content type of "application/vnd.lig".

```lig
<entity> @<attribute> 0xCAFE
_ _ "Yolo"
```

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If addition failed...

If addition succeeded...

### Query Dataset
POST the following.

`/datasets/datasetName`

With the following body and a content type of "application/vnd.wander".

```wander
TODO
```

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If query failed...

If query succeeded...

A list of Statements encoded into Lig.

```lig
<entity23> @<attribute> "Hello"
<entity54> @<attribute> "Cruel"
<entity753> @<attribute> "World"
```
