# TensorTree architecture

This repository focuses on a single, practical example of the semantic memory subsystem under [src](../src), centered on the KnowledgeBase workflow in [src/examples/cli_kb_demo](../src/examples/cli_kb_demo).

The example demonstrates how the memory layer can be used to:

- create a nested category hierarchy,
- insert items into a category,
- list categories,
- run path-based searches,
- and rely on vectorization through the Vectorize API to make semantic retrieval work without a heavy reindexing pass.

The current repository layout is intentionally simple: one example, one entrypoint, and one persistent data directory for the demo.
