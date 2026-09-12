---
name: gloo-mesh-search-product-docs
description: >-
  Answer a Gloo Mesh (Gloo Platform APIs) question from Solo.io's own documentation using the
  public Docs MCP server, then read the full page when the snippet is not enough.
api: Gloo Mesh Docs MCP
generated: '2026-09-12'
method: generated
source: mcp/gloo-mesh-mcp.yml (tool names and arguments read live from https://search.solo.io/mcp on 2026-09-12)
operations:
  - search
  - get_chunks
  - get_full_page
---

# Search the Gloo Mesh documentation

Solo.io runs a remote MCP server at `https://search.solo.io/mcp`. It needs **no credential**.
Three tools; their input schemas are saved verbatim in `mcp/gloo-mesh-mcp-tools.json`.

## Connect

```
claude mcp add --transport http soloio-docs-mcp https://search.solo.io/mcp
```

## Steps

1. **`search`** — required `query` (string, max 2000 chars) and `product` (enum). For this
   product pass `product: "gloo-mesh-enterprise"`. Optional `limit` (default 4).

   Pass the *user's* wording as the query; this is semantic search, not keyword matching.
   Results carry a score, the product, the doc version, the section hierarchy, the **source
   URL** and the **collection** name — keep the last two, the other tools need them.

2. **Judge the snippet before fetching more.** If it already answers the question, stop and
   cite the `Source:` URL. Do not chain tool calls for tidiness.

3. **`get_chunks`** — only when a hit is clearly relevant but truncated. Required
   `collection`, `url`, `startIndex`, `endIndex`. Use the `Collection` and `Source` values
   from the search result, and the `Chunk Index` / `Total Chunks` fields to pick a range.
   This is the cheap way to read the rest of a page.

4. **`get_full_page`** — required `url`. Use when chunking is insufficient. Pages over
   20,000 characters return an error with an approximate token count; pass `force: true`
   only when you truly need the whole thing.

## Rules

- `product` is required and constrained. Gloo Mesh is `gloo-mesh-enterprise`. The renamed
  service-mesh product is `solo-enterprise-for-istio`; they are **different doc sets** and
  searching the wrong one is the most common way to get a confidently wrong answer.
- Version matters. Results carry a `Version:` field (`main`, `2.14.x`, …). `main` is the
  unreleased beta — do not present `main` content as current behaviour. Current stable is
  2.14 (released 2026-09-09); Solo supports n-3.
- Always cite the `Source:` URL back to the user. Every docs page also has a markdown twin:
  append `.md` to any `docs.solo.io` URL.
- The whole documentation index is also available as a plain file:
  `https://docs.solo.io/gloo-mesh-enterprise/llms.txt` (saved as `llms/gloo-mesh-llms.txt`).
- This server searches **documentation**. It cannot read, create or change anything in a Gloo
  Mesh installation. For that, see `gloo-mesh-apply-a-policy`.
