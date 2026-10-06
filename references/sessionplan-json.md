# Sessionplan JSON reference

Use the shared format guide at https://sessionplan.de/import-schema.md and the
canonical JSON Schema at https://sessionplan.de/data/sessionplan.schema.json
when constructing the `session` argument for `create_session_link`.

The live `create_session_link` input schema is authoritative if it differs from
the public reference. Always pass the complete session object, never a patch.
Use this input contract for both new and revised sessions. The decoder response
schema and server compatibility handling are not generation guidance.

Operational rules:

- Use `date: null` and `startTime: null` when those values are unknown.
- Include `version`, `workshop`, `blockTypes`, `persons` and `items`. Use `"1.0"` for the version and `[]` for unused category/person lists.
- Include workshop `title` and `description`; each item includes `id`, `type`, `blockTypeId`, `title`, `duration`, `notes` and `material`. Use `""` for unused text fields and `null` for an unassigned block type.
- Keep item IDs unique across the complete tree and category/person IDs unique within their respective lists. Include `id`, `name` and `color` for each category and person.
- Ensure every `blockTypeId`, `personIds` and `lanePersons` reference points to an actually declared record.
- Use `group` only for sequential nested phases and `breakout` only for parallel work.
- Only containers have a `children` array: groups contain blocks or breakouts; breakouts contain blocks. Blocks omit `children` entirely.
- Preserve IDs and untouched fields when revising a decoded session.
- Return the generated `structuredContent.link` verbatim; never reconstruct or edit it.
