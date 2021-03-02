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

With the following body and a type of "text/ligature".

`#23 attribute "Hello"`

To add a new entity use `_`

`_ attribute "Hello"`

If you put two `_`s then two new entities will be created.

`_ attribute _`

This is a "feature bug" since you won't run into needing to describe a new entity in terms of a relationship
to itself often, and the Wander api allows for this.

Returns:
TODO

### Match Dataset
GET the following.

`root/?entity=##&attribute=##&value=##`

All the attributes are optional.

Returns:
TODO

### Match Dataset with Range

`root/?entity=##&attribute=##&value=##..##`

All the attributes are optional except the value attribute.

Returns:
TODO

## Wander API

Under development.
