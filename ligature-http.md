# Ligature HTTP API
Although implementations may provide embeddable libraries,
the main way Ligature is intended to be interacted with is via HTTP.
Note in the URLs snippets below anything wrapped in `{}` is user defined.

### Get list of Datasets
GET `/datasets`

### Create Dataset
POST `/datasets/{datasetName}`

Returns:

If invalid Dataset name...

If Dataset exists...

If creation failed...

If creation succeeded...

### Delete Dataset
DELETE `/datasets/{datasetName}`

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If deletion failed...

If deletion succeeded...

### Get all Statements from Dataset
GET `/datasets/{datasetName}/statements`

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If fetch failed...

If fetch succeeded...

### Add Statements to Dataset
POST `/datasets/{datasetName}/statements`

With the following body and a content type of "application/vnd.dlig".

```lig
<entity> <attribute> 0xCAFE
^^ "Yolo"
```

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If addition failed...

If addition succeeded...

### Delete Statements from Dataset
DELETE `/datasets/{datasetName}/statements`

With the following body and a content type of "application/vnd.dlig".

```lig
<entity> <attribute> 0xCAFE
^^ "Yolo"
```

Returns:

If invalid Dataset name...

If Dataset doesn't exist...

If addition failed...

If addition succeeded...

### Query Dataset
POST `/datasets/{datasetName}/wander`

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
<entity23> <attribute> "Hello"
<entity54> <attribute> "Cruel"
<entity753> <attribute> "World"
```
