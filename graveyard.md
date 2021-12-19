# 👻🧟‍♀️💀🦇🧛‍♀⚰️Idea Graveyard⚰️🧛🦇💀🧟👻

## Purpose

This document exists as a catalog of some ideas that have been experimented with and eventually were decided against
regarding the design of Ligature.

### Implementing RDF

At various times Ligature was much closer to being a standard RDF implementation.
Since there already are multiple RDF implementations for most popular programming languages,
I've decided to not focus on implementing RDF itself,
but instead of focus on learning from RDF and trying to fix what I see as some issues.

### Anonymous Entities

One design of Ligature focused on all Entities being anonymous.
They would be assigned a unique 64-bit integer as their id when created.
The idea was that the common names for entities would be domain specific,
so some would have URL attributes while others could just have name attribute while other would remain anonymous.
This made referring to Entities difficult and required calling a newEntity method whenever a new Entity was needed.
It also made sharing Entities across Datasets very challenging since there would be many id collisions.

### Having separate types for Statements and PersistedStatements

At one point Ligature contained both a Statement type and PersistedStatement type.
The Statement type contained an Entity, an Attribute, and a Value.
The PersistedStatement type contained a Statement and an Entity representing the Context.
You would add Statements to a Dataset and a PersistedStatement with a unique Context was created.
This however causes an issue when importing since you need to have a specific Context.
Adding a addPersistedStatement method would help but there would still be issues with accidental Context collisions.
Moving to using UUIDs instead of a counter for generating ids is what helped with this problem the most.

### Allowing shared Contexts in a Dataset

After getting rid of PersistedStatements, I thought for a while about allowing Contexts to be shared with multiple Statements.
This is similar to RDF quads where multiple Statements can belong to the same named graph.
I decided against this since it is confusing to have a single feature (Contexts in this case)
that can be used for multiple things (either a unique pointer to a Statement OR something similar to a named graph).
Instead, all Contexts must be unique now and if you want named graph like behavior
then you can use the Context to say that a number of Statement belong to a group.

### Using JSON to serialize Ligature Statements

Originally the Atomic API entirely used JSON.
Eventually I decided that representing Statements with JSON was a pain, and I created the Lig serialization format.
The main issues were the amount of Strings that had to be used throughout the serialization.
Entities, Attributes, Contexts, Bytes, Strings, and Integers all had to be stored as Strings,
and the type of Value had to be encoded as a String as well.
Switching to Lig has so far be a large improvement, and it isn't a complicated format so porting should not be an issue.

### Allowing comments in Lig files

Adding comments to a serialization format is an easy addition and seems like it could be useful
for things like describing an ontology or something else complex.
However, Lig is not intended to be worked with directly often.
I think it is good to be human-readable but editing by hand is not something I want to encourage.
Editing Lig by hand is especially difficult since unique Contexts must be supplied for every Statement.
Instead, tools or scripts should be used, and documentation should be encoded within the data model itself.

### Allowing wildcards in Lig files

Wildcards were an idea I had to make lig files slightly more compact by allowing users to point to the value
referenced in the Statement above.  For example

```lig
<a> @<test> <example> <context1>
<b> _ _ <context2>
```

Is equivalent to

```lig
<a> @<test> <example> <context1>
<b> @<test> <example> <context2>
```

For similar reasons for removing comments, I've decided to remove wildcards.
For one thing it seemingly encourages people to work directly with lig which isn't something I want to encourage.
The space-saving is pretty much irrelevant.
It adds ordering dependencies to lig files as well as makes it difficult to copy and paste from lig files.

*Note: This is being added to a variation of Lig called DLig*

### Embedding a 3rd party scripting language instead of implementing Wander

Several projects similar to Ligature embed a 3rd party language (usually JavaScript, but I was also considering Lua).
I've decided against this since I want Ligature to be easy to implement in multiple languages and any particular
language may not have bindings for an embedded language.
Also sandboxing becomes a big issue with embedding 3rd party languages.
Finally, by implementing a custom language primitives for Ligature can be given first class support.

### Having Separate Types for Entities and Attributes

For a while Ligature had separate types for both Entities and Attributes.
This seemed like overkill and would make working on SOL harder.
Instead, I've decided to use have an Identifier type.

### Supporting Floating Point Numbers Natively In Ligature

Floating Point values were an original literal type.
After working on the Rust implementation and thinking about it, I realized that I should put off supporting Floating Point numbers.
How they are sorted or compared is very much in the air and can be context-sensitive.
For most of my initial case studies I don't need Floating Point numbers and where I need them I can encode them as Bytes.
This will be revisited I'm sure but for now I'm just leaving them out.

### Graph datatype in Wander

At one point I was planning on adding a datatype to Wander call Graph that was going to basically be a non-transaction,
in-memory Ligature instance.
I've decided to drop this for now since I really can't think of very many good use cases for it, but I'm willing
to revisit this decision.
Its introduction could also cause since issue since Wander doesn't have function overloading right now,
so some of the function names would have to be longer or I'd need to implement function overloading.
