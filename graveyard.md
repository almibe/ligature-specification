# Idea Graveyard

## Purpose

This document exists as a catalog of some ideas I've experimented with and eventually decided against
regarding Ligature.

### Implement RDF

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

### Not enforcing that Contexts are unique in a Dataset

After getting rid of PersistedStatements, I thought for a while about allowing Contexts to be shared with multiple Statements.
This is similar to RDF quads where multiple Statements can belong to the same named graph.
I decided against this since it is confusing to have a single feature (Contexts in this case)
that can be used for multiple things (either a unique pointer to a Statement OR something similar to a named graph).
Instead, all Contexts must be unique now and if you want named graph like behavior
then you can use the Context to say that a number of Statement belong to a group.

### Using JSON to serialize Ligature data

...

### Allowing comments in Lig files

Adding comments to a serialization format is an easy addition and seems like it could be useful
for things like describing an ontology or something else complex.
However, Lig is not intended to be worked with directly often.
I think it is good to be human-readable but editing by hand is not something I want to encourage.
Editing Lig by hand is especially difficult since unique Contexts must be supplied for every Statement.
Instead, tools or scripts should be used.
