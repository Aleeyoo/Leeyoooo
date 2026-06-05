---
status: processed
wiki: "[[WIKI/PydanticAgent记忆]]"
---

# Pydantic fixed my Agent's Memory

Your agent remembers everything and understands nothing.

Agent memory started with vector databases. Store facts as chunks, retrieve by similarity.

It works until a query needs to connect facts across chunks. Then it falls apart. The problem isn't similarity. It's structure.

Knowledge graphs were the fix. Entities as nodes, relationships as edges, traversal instead of matching.

But most teams hit a different wall.

When you give an agent a knowledge graph for memory, the default behavior is that the LLM handling extraction decides the structure on its own.

It picks the entity types, the relationship labels, and the attributes.

The results are generic.

For example, you’re building a customer support agent. You feed it 50 support conversations covering customers, tickets, features, and escalation history.

You ask: *“Which enterprise customers have open sev-1 tickets?”*

The graph has the data. But every support ticket is stored as a “Topic” node. Every customer is an “Object.” Every relationship is “RELATES_TO.”

There’s no way to filter by type, severity, or plan tier. The query returns noise.

The agent didn’t forget anything. Nobody told it what to pay attention to.

![](https://pbs.twimg.com/media/HJKtp0JaAAA0zu9.jpg)

The fix is straightforward: **define the schema upfront.** Tell the extraction model what types of entities exist in your domain, what relationships are valid, and what attributes each one carries.

That organizational blueprint is called an **ontology**. Think of it as the **schema for your agent’s brain**.

Let’s walk through why this matters, what breaks without it, and how to implement it it using a [**100% open-source solution**](https://github.com/getzep/graphiti).

# Why flat retrieval breaks on multi-hop reasoning

Vector-based memory stores facts as text chunks and retrieves them by semantic similarity. That works until a query requires connecting facts that don’t appear in the same chunk.

Consider three facts stored about a project.

- Alice manages Project Atlas
- Project Atlas runs on PostgreSQL
- The PostgreSQL cluster went down Tuesday
A query like “was Alice’s project affected by Tuesday’s outage” needs all three.

![](https://pbs.twimg.com/media/HJKu6QBaYAAINyy.jpg)

Vector search will retrieve just facts 1 and 3 because both mention relevant terms. Fact 2 is the bridge connecting Alice to PostgreSQL through Project Atlas, but it mentions neither Alice nor Tuesday. Similarity search misses it.

A knowledge graph stores entities as nodes and relationships as edges. Instead of matching text, it traverses connections.

That chain (Alice → manages → Project Atlas → runs on → PostgreSQL) is what makes multi-hop reasoning work, and it is invisible to flat vector retrieval.

# The memory pipeline and where extraction fits

Every graph-based agent memory system follows a common pipeline:

1. **Ingest:** Raw data comes in (conversation messages, documents, JSON business data)
2. **Extract:** An LLM reads the raw data and decides what entities exist, what relationships connect them, and what attributes matter
3. **Store:** Extracted entities become nodes, relationships become edges, all persisted in the graph
4. **Retrieve:** At query time, the system searches the graph and assembles relevant facts
5. **Deliver:** Retrieved facts are formatted into a context block and injected into the agent’s prompt
The extraction step is where everything is decided. It determines what your graph contains, how it’s structured, and what’s queryable downstream.

![](https://pbs.twimg.com/media/HJKu_uTbUAAtvP6.jpg)

Here’s the problem. In most frameworks, this step is a black box. You pass in text, an LLM pulls out “entities” and “relationships,” and you get nodes and edges. The LLM decides the types, the labels, the attributes on its own.

You have zero control over what it classifies or how.

Let's understand how to fix it.

# [Defining the schema with Pydantic](https://github.com/getzep/graphiti)

The fix is the same pattern used everywhere in the AI stack.

- FastAPI endpoints get Pydantic response models.
- Function calling tools get Pydantic schemas.
- Agent memory works the same way in Zep.
Define custom entity types using EntityModel (a subclass of Pydantic’s BaseModel) with EntityText fields and descriptions that guide the extraction model.

The docstrings and field descriptions are important here because good descriptions with concrete examples give the extractor enough signal to classify accurately.

The Pydantic descriptions above aren’t just classification instructions. They teach the extractor vocabulary it doesn’t know.

A Technology entity follows the same pattern.

Edge types use EdgeModel and carry their own attributes.

Finally, wire these into the graph with source/target constraints using EntityEdgeSourceTarget, which defines which entity types can connect through which edge types:

The code enforces that

- WORKS_ON can only connect a User to a Project
- USES_TECHNOLOGY can only connect a User to a Technology.
- Any relationship that doesn't match these constraints won't produce a typed edge.
To summarise, this is what we’ve got so far:

![](https://pbs.twimg.com/media/HJKwmn9a4AAv9DP.jpg)

# What happens under the hood

When a conversation is ingested with a schema active, Zep’s extraction pipeline runs five steps:

1. **Entity extraction** identifies named entities in the text
2. **Entity resolution** merges duplicates (”Nexus” and “the Nexus project” become one node)
3. **Fact extraction** identifies relationships and outputs them as typed edges
4. **Fact resolution** detects contradictions and invalidates outdated facts (preserving history)
5. **Temporal extraction** parses time references and maps them to validity windows on each edge
[📹 video](https://video.twimg.com/tweet_video/HJKyJvDakAA1jt8.mp4)

Your pydantic schema guides steps 1 and 3. Entity types tell the extractor what to look for. Edge types with their constraints tell it what relationships to classify. Resolution and temporal processing happen automatically.

# Practical walkthrough of how it looks

We ingest a conversation where a developer named Alex discusses their work (an active web app called Nexus, their tech stack, proficiency levels):

![](https://pbs.twimg.com/media/HJKw-WEbsAAFsIL.jpg)

Querying for Project nodes returns Nexus with populated project_status and project_type attributes.

![](https://pbs.twimg.com/media/HJKxDS3aQAA4y2G.jpg)

The node isn’t a generic “Topic” or “Object.” It’s a Project with structured fields as defined in the schema.

The edges are typed too.

- WORKS_ON carries role: lead developer
![](https://pbs.twimg.com/media/HJKxGAibMAA6XE-.jpg)

- USES_TECHNOLOGY carries proficiency: advanced for Python and Docker, proficiency: intermediate for TypeScript.
![](https://pbs.twimg.com/media/HJKxJ56bEAAfcUz.jpg)

This can now filter projects by status, technologies by category, and query “which active projects use PostgreSQL” with a precise answer.

# Context templates

The final piece is context templates, which assemble typed facts into a prompt-ready block.

You can define which edge types and entity types to include, and Zep formats them with temporal annotations into a single string injected into the agent’s prompt.

It looks like this:

![](https://pbs.twimg.com/media/HJKxWNMacAET_un.jpg)

Every entry in the resulting context block is typed, temporally annotated, and carries the attributes defined. Save the template once, reference it by ID in agent calls.

# The 10/10/10 constraint and schema as a reasoning boundary

Zep enforces a hard limit of 10 custom entity types, 10 custom edge types, and 10 fields per type.

![](https://pbs.twimg.com/media/HJKxcHNboAEfD0H.jpg)

That’s intentional to force a dev to think about what matters in a domain rather than modeling everything.

The source/target constraints also act as guardrails on what an agent is allowed to remember. If a schema doesn’t include an edge type connecting Project to Competitor, the extraction model won’t create that relationship, even if a conversation mentions both.

The schema defines the space of valid memories.

This is the same principle behind typed function calling, where we constrain the LLM’s output space so that it can’t produce invalid arguments. Memory schemas apply that same constraint to what the agent stores.

Start with 3-4 entity types and 3-4 edge types that capture 80% of your domain logic, and add complexity incrementally.

Agent memory without schema discipline is a graph that behaves like a vector store.

In a way, you pay the cost of graph construction without getting the benefit of structured retrieval.

The schema is how you get that benefit back, and the fact that it’s Pydantic means there’s nothing new to learn.

This is especially true for domain-specific applications. LLM extraction works reasonably well on general knowledge, but the moment your domain has internal terminology, product names that collide with common words, or jargon absent from the training data, unguided extraction produces nonsense. The schema closes that gap. It carries the domain vocabulary directly into the extraction step, so the LLM doesn't need to have seen your terminology before. It just needs the definitions you wrote.

![](https://pbs.twimg.com/media/HJK0eA6bIAAfZ8w.jpg)

[**You can find Zep’s GitHub repo here →**](https://github.com/getzep/graphiti) (don’t forget to star [https://abs.twimg.com/emoji/v2/svg/1f31f.svg](https://abs.twimg.com/emoji/v2/svg/1f31f.svg)�)

Thanks for reading!

https://x.com/akshay_pachaar/article/2058976178908885210

— [Akshay (@akshay_pachaar)](https://x.com/akshay_pachaar/status/2058976178908885210) · 2026-05-26 02:19
