# Agent guidance

Treat this repository's central Signal → Nexus → SEMA actor and socket
topology as wired legacy behavior, not design law. The approved target has
each component daemon owning its own embedded sema database. A separate small
translator daemon is authoritative only for shared naming and encodedID
allocation.

Reuse tested actor, transaction, subscription, and recovery mechanisms where
they fit, but do not extend the central document-storage or stateless-client
model. The repository's eventual dissolution and the translator daemon's
final name remain undesigned; do not imply either migration has landed.

This repository is under fast development and constantly breaking.
