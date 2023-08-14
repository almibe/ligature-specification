# 👻🧟‍♀️💀🦇🧛‍♀⚰️Idea Graveyard⚰️🧛🦇💀🧟👻

## Purpose

This document exists as a catalog of some ideas that have been experimented with and eventually were decided against
regarding the design of Ligature.

I started this document before I started doing more formal ADRs, so this document can be considered retired.

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

*Note: The example above uses the removed attribute syntax with the `@` character.*

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

### Having a fourth `Context` element in Statements

From the beginning of Ligature I really wanted it to be easy to make Statements about Statements.
My first approach to this was adding a fourth element to all Statements called the `Context`.
As I've gotten further along in developing Ligature I've come to the conclusion that the `Context` isn't needed and actually makes a lot of thing worse.

Basically:

 * It makes representing Ligature as a graph difficult and very noisy.
 * It forces you to make an identifier for all Statements even when you never plan on using them.
 * Makes interop with other graph-like data structures more difficult.
 * It doesn't really work as well I thought in practice
  * If you are assigning multiple attributes at once and they are intended to be grouped together you have to apply metadata to all of them separately or create a metadata node (which has its own context).
 * Typing contexts is weird (both physically and applying classes to contexts)
 * Might be a weird idea to explain to people?
 * I haven't even started to think about how contexts work with schema and ontologies (probably not well)

Instead I plan on exploring using concepts from event sourcing to do this type of work.
Below is an example of representing projects with main contacts.
It's a little convoluted but I think it represents the basic change well.

```
<person:1> <name> "Bob" <context:1>
<person:1> <email> "bob@fake.com" <context:2>
<person:2> <name> "Robin" <context:3>
<person:2> <email> "robin@fake.com" <context:4>
<project:1> <project:contact> <person:1> <context:5>
<context:5> <date> "1999/07/13" <context:6>
<project:1> <project:contact> <person:2> <context:7>
<context:7> <date> "2020/07/12" <context:8>
```

32 Nodes

```
<person:1> <name> "Bob"
<person:1> <email> "bob@fake.com"
<person:2> <name> "Robin"
<person:2> <email> "robin@fake.com"
<project:1> <project:contact> <contactEvent:1>
<contactEvent:1> <name> <person:1>
<contactEvent:1> <date> "1999/07/13"
<project:1> <project:contact> <contactEvent:2>
<contactEvent:2> <name> <person:2>
<contactEvent:2> <date> "2020/07/12"
```

30 Nodes

Ideally I'd also have events for assigning names and email addresses to people and not use strings for dates, but this is an example.
What I want to point out is that every node in the second example is important while in the first many aren't referenced.
Also adding metadata about name and email assignments would result in just as many unused nodes.
In realistic datasets I feel like the burden of contexts would be even worse.

### Having Two Text-Based Serialization Formats

For a while I had two different text-based serialization formats.
The first was very basic and focused on just representing Statements as simply as possible.
The other allowed for repeating Entities and Attributes easily, generating random IDs, and allowed using prefixes to shorten input.
I've decided to combine the two formats for the sake of simplicity.

### Having a Ligature HTTP spec

I originally thought that HTTP would be the main way that people work with Ligature.
This might be true, but for now I've decided to just have each application solve this problem
in a way that makes sense for the specific application.
Later I can revisit this once I have more experience with using Ligature.
