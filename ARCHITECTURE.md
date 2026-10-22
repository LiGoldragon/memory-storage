# memory-storage architecture

## Current implementation: wired legacy topology

This repository currently implements a standalone central daemon. Requests
traverse state-bearing Kameo actors in Signal → Nexus → MEMORY order; MEMORY owns
one `memory_engine::Engine`, and Nexus fans committed changes to push
subscribers. Component daemons currently reach it through typed sockets.

That is the wired legacy topology, not the approved architecture. The
approved target has every component daemon stateful and owning its own
embedded memory database. A separate small translator daemon is authoritative
only for shared naming and encodedID allocation. It does not become a central
store for Ethos, Nomos, or Logos documents.

The storage migration has not landed. The old topology is retired only after
component-by-component migration; its detailed staging and this repository's
eventual dissolution remain open design work.

## Existing identity implementation: mechanism donor

The current code stores `UniverseRegistry`, `UniverseState`, `DeclaredKey`,
`TypeIdentity`, `SchemaWholeHandle`, and `DeclaredShape` records and exposes
`BindIdentities`, `MintOrBind`, and `Continue`. Tests cover atomic commits,
write-nothing rejection, restart recovery, and monotonic allocation.

Those types and their flat identity ontology are superseded and must not guide
the translator contract. The approved naming model uses variant-fronted,
nested module-owned nametables; full chains of module-local encodedIDs are
durable identity. Declarations allocate, references only resolve, and
spelling-only operational rename is the sole identity-preserving rename.
Tested serialization, transaction, actor, subscription, restart, and recovery
mechanisms may be reseated behind that model.

## Current implementation notes

- One closed typed record family stores document versions and allocator
  cursors.
- The Unix socket and `/tmp/new-language-engine` defaults isolate prototype
  state; production Spirit paths are never used.
- Fixture allocation remains monotonic per explicit scope in the current
  code. It is compatibility behavior, not the target identity model.
- Existing central document and allocator records describe what the wired
  daemon can read today. Do not extend them as a substitute for per-daemon
  embedded state.
