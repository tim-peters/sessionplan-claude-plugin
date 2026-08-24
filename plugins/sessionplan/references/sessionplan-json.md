# Sessionplan JSON reference

Use the shared public schema at https://sessionplan.de/import-schema.md when
constructing the `session` argument for `create_session_link`. It is also the
schema used by Sessionplan's manual JSON import and REST API.

The live MCP tool schema is authoritative if it differs from the public
reference. Always pass the complete session object, never a patch.

Operational rules:

- Use `date: null` and `startTime: null` when those values are unknown.
- Keep every item, block type, and person ID unique within the session.
- Ensure every `blockTypeId` and `personIds` reference an actually declared record.
- Use `group` only for sequential nested phases and `breakout` only for parallel work.
- Preserve IDs and untouched fields when revising a decoded session.
- Return the generated `structuredContent.link` verbatim; never reconstruct or edit it.
