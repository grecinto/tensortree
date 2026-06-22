# TensorTree

TensorTree is a developer-friendly approach to semantic memory built on top of SOP’s KnowledgeBase architecture.

## Repository layout

- `docs/architecture.md` — high-level architecture notes
- `src/examples/cli_kb_demo/` — the single runnable KnowledgeBase example
- `src/` — source snapshot from the SOP memory subsystem

## Example

The repository currently includes one runnable example:

- `src/examples/cli_kb_demo/main.go` — creates nested categories, inserts items, lists categories, and searches by path using the filesystem-backed KnowledgeBase wrapper.

Run it with:

```bash
go run ./src/examples/cli_kb_demo
```

The example writes its demo data under `./data/demo-cli-kb` and can be reset by removing that directory.

## TensorTree In-Depth

Instead of treating memory as a flat pile of vectors, TensorTree lets you organize knowledge into meaningful categories, nest those categories into a hierarchy, and retrieve content through either path-based navigation or semantic similarity.

A key part of making semantic search work is vectorization. TensorTree uses the Vectorize API to turn content into embeddings on demand, so the system can compare meaning without requiring a heavy reindexing workflow. In practice, this means a simple vectorization step can be applied to new content, and the resulting vectors are used immediately for semantic retrieval.

A key idea behind TensorTree is that categories are not just labels for retrieval—they are inherently visualizable. In SOP, KnowledgeBases can be surfaced through the SOP Data Manager, where Spaces add a visual layer that makes the category graph and its relationships easier to explore, understand, and reason about.

This makes it a strong fit for:

- RAG systems
- copilots and agents
- documentation search
- internal knowledge tools
- domain-specific AI assistants

## Why TensorTree?

Most vector databases are great at similarity search, but they can feel too low-level for application developers. TensorTree adds structure to the experience:

- categories act as semantic anchors,
- items live under those categories,
- nested paths make the knowledge base readable and explainable,
- retrieval can be guided by both taxonomy and meaning.

The result is a semantic memory layer that is easier to reason about and easier to build with.

## Getting started

TensorTree can be used from Go with a simple KnowledgeBase flow:

1. create a KnowledgeBase,
2. define categories,
3. upsert items into those categories,
4. list or search them.

```go
kb, err := database.NewKnowledgeBase(ctx, "demo-kb", sop.DatabaseOptions{
    Type:          sop.Standalone,
    StoresFolders: []string{"./data/demo-kb"},
}, nil, nil, false)
```

From there, you can create a category tree, add content, and retrieve relevant items with a small, high-level API.

## Key idea

TensorTree is not just a vector store. It is a practical semantic memory layer that helps AI applications remember and retrieve information in a way that is structured, navigable, and developer-friendly.

The important usability point is that semantic search is powered by a simple Vectorize step, not by a heavyweight reindexing pipeline. That keeps the workflow approachable while still enabling meaning-based retrieval.

If you want a simple way to bring structured semantic memory into your AI app, TensorTree is a strong place to start.
