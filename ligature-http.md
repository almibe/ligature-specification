# Ligature HTTP API
Although implementations may provided embeddable libraries,
the main way Ligature is intended to be interacted with is via HTTP.
Ligature supports two different APIs for working with HTTP.
The first is called Atomic, and it performs a single action per HTTP request.
It exists as a minimal API for working with Ligature and also a convenience when you just want to do some stuff.
The other API is based on sending Wander applications to the server and isn't complete yet.

## Atomic API
As stated above the goal of the Atomic API is to be easy to implement and allows you to do one thing at a time.
This probably won't be very useful once the Wander API is complete, but for now it allows you to work easily with Ligature.

### Create Dataset
POST the following.

`root/datasetName`

Returns:
TODO

### Delete Dataset
DELETE the following.

`root/datasetName`

Returns:
TODO

### Add Statement to Dataset
POST the following.

`root/datasetName`

With the following body and a type of "application/json".
NOTE: for the sake of simplicity all values are strings (unless it's a null Entity - see below),
and the value-type attribute tells us how to parse it.

```json
{
  "entity": "23",
  "attribute": "attribute",
  "value": "Hello",
  "value-type": "StringLiteral"
}
```

To add a new entity use null

```json
{
  "entity": null,
  "attribute": "attribute",
  "value": "Hello",
  "value-type": "StringLiteral"
}
```

If you put two nulls then two new entities will be created.

```json
{
  "entity": null,
  "attribute": "attribute",
  "value": null,
  "value-type": "Entity"
}
```

This is a "feature bug" since you won't run into needing to describe a new entity in terms of it have a
relationship to itself often, and the Wander api allows for this.

Returns:
TODO

### Match Dataset
GET the following.

`root/?entity=##&attribute=##&value=##`

All the attributes are optional.

Returns:

A list of Statements crudely encoded into JSON.
See the explanation of encoding above in the "Add Statement to Dataset" section.

```json
[
  {
    "entity": "23",
    "attribute": "attribute",
    "value": "Hello",
    "value-type": "StringLiteral"
  },
  {
    "entity": "54",
    "attribute": "attribute",
    "value": "Cruel",
    "value-type": "StringLiteral"
  },
  {
    "entity": "753",
    "attribute": "attribute",
    "value": "World",
    "value-type": "StringLiteral"
  }
]
```

### Match Dataset with Range

`root/?entity=##&attribute=##&start-value=##&end-value=##`

All the attributes are optional except the start-value and end-value attributes.

Returns:
TODO

## Wander API

Under development.
