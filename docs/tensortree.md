# TensorTree: A Practical Guide to Building Semantic Memory with SOP

If you have ever built a retrieval system, a copilot, or an AI assistant, you already know the hard part is not connecting to an LLM. The hard part is making sure the right knowledge is found at the right time.

That is where TensorTree comes in.

TensorTree is a developer-friendly way to think about SOP's new KnowledgeBase-powered semantic memory layer. It brings together three ideas that matter most for modern AI apps:

- hierarchical knowledge organization,
- semantic retrieval,
- and a simple API that feels natural to use.

Instead of treating memory as a flat bag of vectors, TensorTree lets you organize knowledge into meaningful categories, store items beneath those categories, and retrieve them through either a path-based lookup or semantic matching.

In short: TensorTree makes it easy to build AI memory that is understandable, navigable, and practical.

---

## Why TensorTree exists

Most traditional vector databases work well for similarity search, but they often feel too low-level for application developers. You have to think about embeddings, indexes, clustering, chunking, namespaces, and search mechanics all at once.

TensorTree takes a simpler approach.

It is built on top of SOP's KnowledgeBase model, which adds structure to vector search:

- categories act as semantic anchors,
- items live under those categories,
- nested paths give the system a human-readable hierarchy,
- search can be routed through the taxonomy instead of relying only on raw vector similarity.

That means TensorTree is not just a vector store. It is a semantic knowledge layer.

---

## What makes TensorTree different

TensorTree is designed around a few principles that make it especially appealing for developers building AI products.

### 1. It is structured, not flat

Instead of storing everything in a single unstructured vector space, you create a hierarchy of categories. That makes the knowledge base easier to navigate, easier to debug, and easier to reason about.

A typical structure might look like:

- Root
  - Engineering
    - Memory
    - Architecture
  - Product
    - Roadmaps
    - Customer Feedback

That hierarchy gives both humans and AI agents a clear way to organize information.

### 2. It supports semantic and path-based retrieval

You can search by:

- exact category path,
- semantic similarity,
- or a combination of both.

This makes it useful for real-world workflows such as:

- RAG systems,
- copilots,
- knowledge assistants,
- internal documentation search,
- domain-specific agents.

### 3. It is local-first and practical

TensorTree is built for real applications. It can run locally, persist data on disk, and work without forcing developers to stand up a large external infrastructure stack.

That makes it attractive for:

- prototypes,
- local experiments,
- CLI tools,
- desktop apps,
- internal AI tools,
- edge or offline-friendly systems.

### 4. It is easy to use

The biggest design goal of TensorTree is simplicity. You should not need to manually assemble a dozen storage primitives just to get started.

A single constructor and a handful of intuitive methods are enough to begin building a real semantic memory layer.

---

## Getting started in Go

The easiest way to begin is with SOP's KnowledgeBase entrypoint.

Here is the core flow:

1. create a KnowledgeBase,
2. define categories,
3. upsert items into those categories,
4. list categories,
5. run a search.

### Example: a tiny semantic memory app

```go
package main

import (
    "context"
    "fmt"

    "github.com/sharedcode/sop"
    "github.com/sharedcode/sop/ai/database"
    "github.com/sharedcode/sop/ai/memory"
)

func main() {
    ctx := context.Background()

    kb, err := database.NewKnowledgeBase(ctx, "demo-kb", sop.DatabaseOptions{
        Type:          sop.Standalone,
        StoresFolders: []string{"./data/demo-kb"},
    }, nil, nil, false)
    if err != nil {
        panic(err)
    }
    defer kb.Close(ctx)

    err = kb.UpsertCategories(ctx, []memory.UpsertCategoryParam{{
        Category: &memory.Category{ID: sop.NewUUID(), Name: "Root", Path: "Root"},
    }})
    if err != nil {
        panic(err)
    }

    err = kb.UpsertCategories(ctx, []memory.UpsertCategoryParam{{
        Category:    &memory.Category{ID: sop.NewUUID(), Name: "Engineering", Path: "Root/Engineering"},
        ParentPaths: []string{"Root"},
    }})
    if err != nil {
        panic(err)
    }

    err = kb.UpsertCategories(ctx, []memory.UpsertCategoryParam{{
        Category:    &memory.Category{ID: sop.NewUUID(), Name: "Memory", Path: "Root/Engineering/Memory"},
        ParentPaths: []string{"Root/Engineering"},
    }})
    if err != nil {
        panic(err)
    }

    err = kb.UpsertItems(ctx, []memory.UpsertItemParam[map[string]any]{{
        CategoryPath: "Root/Engineering/Memory",
        Item: &memory.Item[map[string]any]{
            Data: map[string]any{"text": "Use KnowledgeBase for semantic memory"},
        },
    }})
    if err != nil {
        panic(err)
    }

    categories, _, err := kb.ListCategories(ctx, memory.ListCategoriesParam{
        ParentPath: "Root/Engineering",
        Limit:      20,
        Offset:     0,
    })
    if err != nil {
        panic(err)
    }

    fmt.Println("Categories:")
    for _, cat := range categories {
        fmt.Printf("- %s (%s)\n", cat.Name, cat.Path)
    }
}
```

That is the core of the experience. You create a mental model for your knowledge, then let the system retrieve it for you when needed.

---

## Why developers like TensorTree

Developers often want three things from an AI memory system:

1. predictable structure,
2. good retrieval quality,
3. straightforward code.

TensorTree delivers all three.

It is especially useful when your app needs to behave more like a knowledge assistant and less like a raw search box.

For example, imagine an internal support bot that must understand:

- product categories,
- troubleshooting domains,
- runbooks,
- known issues,
- architecture notes.

With TensorTree, that knowledge can be organized into a stable hierarchy and retrieved in a way that is easy to explain and easy to maintain.

---

## Where TensorTree fits

TensorTree is a strong fit for:

- RAG applications,
- domain copilots,
- documentation assistants,
- developer tools,
- workflow agents,
- internal knowledge systems,
- semantic memory layers for AI products.

It is also a natural fit when you want a semantic layer that sits close to your application logic rather than being a separate, opaque platform.

---

## The big picture

TensorTree is a reminder that semantic memory does not need to be complicated.

You do not need to start with giant infrastructure or a rigid vector platform. You can begin with a simple idea:

- define the categories that matter,
- store meaningful items beneath them,
- retrieve them using semantic or path-based logic.

That is the promise of TensorTree.

It gives developers a practical way to turn knowledge into a navigable, reusable memory layer for AI systems.

---

## Conclusion

TensorTree is not just another vector database wrapper. It is a developer-first way to build semantic memory that feels structured, explainable, and approachable.

If you are building AI applications that need to remember, retrieve, and reason over information, TensorTree offers a compelling path forward.

The best part is that you can start small and grow naturally.

Create a knowledge base, define a category tree, add your items, and let the system do the rest.

That is the future of practical AI memory: simple enough to build, strong enough to scale, and useful enough to ship.
