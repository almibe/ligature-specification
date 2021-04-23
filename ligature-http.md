# Ligature HTTP API
Although implementations may provide embeddable libraries,
the main way Ligature is intended to be interacted with is via HTTP.
Ligature supports two different APIs for working with HTTP.
The first is called Atomic, and it performs a single action per HTTP request.
It exists as a minimal API for working with Ligature and also a convenience when you just want to do some stuff.
The other API is based on sending Wander scripts to the server and isn't complete yet.

## Atomic API
As stated above the goal of the Atomic API is to be easy to implement and allows you to do one thing at a time.
This probably won't be as useful once the Wander API is complete, but it allows you to work easily with Ligature.

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

### Add Statements to Dataset
POST the following.

`root/datasetName`

With the following body and a type of "application/lig".

```lig
<entity> @<attribute> 0xCAFE <context>
_ _ "Yolo" <context2>
```

Returns:
TODO

### Match Dataset
GET the following.

`root/?entity=##&attribute=##&value=##&value-type=##`

All the attributes are optional.
If you search for a value you must also include the value-type.

Returns:

A list of Statements encoded into Lig.

```lig
<entity23> @<attribute> "Hello" <context124234>
<entity54> @<attribute> "Cruel" <context7125346>
<entity753> @<attribute> "World" <context534563>
```

### Match Dataset with Range

`root/?entity=##&attribute=##&start-value=##&end-value=##&value-type=##`

All the attributes are optional except the start-value, end-value, and value-type attributes.

Returns:

A list of Statements encoded into Lig.

```lig
<entity23> @<attribute> "Hello" <context124234>
<entity54> @<attribute> "Cruel" <context7125346>
<entity753> @<attribute> "World" <context534563>
```

## Wander API

Under development.
