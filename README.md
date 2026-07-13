# Open Source Contribution — AutoGPT

## Project

[AutoGPT](https://github.com/Significant-Gravitas/AutoGPT) — a platform for building, deploying, and running continuous AI agents, including an agent marketplace and library.

**Forked to:** [Kidus5168/AutoGPT](https://github.com/Kidus5168/AutoGPT)

## Selected Issue

**[#9879 — Marketplace data should be downloaded with Agent](https://github.com/Significant-Gravitas/AutoGPT/issues/9879)**

## Problem Summary (Phase I)

When a user downloads an agent from the marketplace into their personal library, the agent shows up with stale information rather than the details it was published with:

1. **Wrong title.** The downloaded agent displays the creator's *original* graph title instead of the title the agent was published under on the marketplace.
2. **Missing images.** The agent's marketplace image is not carried over, so the library entry has no artwork.

**Expected behavior:** A downloaded agent should appear in the user's library exactly as it was presented on the marketplace — with the marketplace title and image intact.

## Codebase Analysis (Phase II)

I traced the marketplace → library download path in the backend and found the root cause: the marketplace title and image are never copied onto the `LibraryAgent` when it is created.

### Where the data lives

The marketplace-facing title and images are stored on the `StoreListingVersion` model:

- `autogpt_platform/backend/schema.prisma` (`model StoreListingVersion`)
  - `name       String`   ← the marketplace publication title
  - `imageUrls  String[]` ← the marketplace images

### Where the download happens

- `autogpt_platform/backend/backend/api/features/library/_add_to_library.py`
  - `resolve_graph_for_library()` already fetches the `StoreListingVersion` (so its `name`/`imageUrls` are in hand).
  - `add_graph_to_library()` creates the `LibraryAgent` from the **graph only**. On the `LibraryAgent.prisma().create(...)` call it connects the `AgentGraph` but **never sets `imageUrl`** and never records the marketplace title.

### Where the wrong data surfaces

- `autogpt_platform/backend/backend/api/features/library/model.py` — `LibraryAgent.from_db()`
  - `name=graph.name` → shows the creator's original graph title, not the marketplace title.
  - `image_url=agent.imageUrl` → null, because it was never populated on download.

### Proposed fix approach

In `add_graph_to_library()`, use the already-resolved `StoreListingVersion` to carry the marketplace data onto the new `LibraryAgent`:

1. **Image** — set `LibraryAgent.imageUrl` from the listing's `imageUrls[0]` at creation time (and on the soft-delete restore path too).
2. **Title** — surface the marketplace title instead of the graph title. Either persist the listing `name` onto the `LibraryAgent`, or have `from_db()` prefer the related store listing's `ActiveVersion.name` over `graph.name` for agents added from the marketplace (`from_db` already reads `store_listing.ActiveVersion.name` for its `MarketplaceListing` sub-object).

**Files that would change:** `_add_to_library.py` (primary), `model.py` (display mapping), plus their existing test files (`_add_to_library_test.py`, `model_test.py`).

## Status

Phase II — codebase investigated, root cause identified, fix approach scoped. Interest comment on the issue still pending; implementation/PR is the next step.
