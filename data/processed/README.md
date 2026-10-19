# SEAC processed knowledge resources

Version: data-v1.0.0. [Download the data release](https://github.com/drinkaBeer/seac-emergency-decision-support/releases/tag/data-v1.0.0).

| Data package | Contents | Size |
|---|---|---:|
| [Evidence store](https://github.com/drinkaBeer/seac-emergency-decision-support/releases/download/data-v1.0.0/seac_evidence_store_v1.0.0.zip) | SQLite database; 306 documents, 58,169 clauses, 109,163 context records and 33,207 evidence links | 79,484,096 bytes |
| [Knowledge graph](https://github.com/drinkaBeer/seac-emergency-decision-support/releases/download/data-v1.0.0/seac_knowledge_graph_v1.0.0.zip) | 133,404 nodes, 255,033 relationships, schema and Cypher restoration data | 64,507,107 bytes |

`manifest.json` lists SHA-256 values, package members, source/publication versions and validation results. `SHA256SUMS.txt` covers the downloadable archives and manifest. `documents.csv` maps document IDs to source files under data/. Its `relative_path` uses the current English directory names; `original_relative_path` retains the path recorded in the versioned evidence database and release archive. Both paths are relative to data/.

## Restore the evidence store

Extract the evidence package, verify checksums, and open evidence_store/evidence.db with SQLite in read-only mode. It is a complete database with FTS indexes. Run PRAGMA integrity_check and PRAGMA foreign_key_check; expect ok and no foreign-key errors.

This is a publication copy, not the original file bytes: operational ingestion/retrieval log rows were removed and free pages vacuumed. Five core tables retain identical sorted logical hashes, and schema version 1.1.0 and artifact_set_hash are preserved. The published database SHA-256 is `9a67e1f4349b8b0e3a30792463657b783267de1d3053e5ffafe7e39a9747ad5f`. An application that pins the original frozen database hash must validate this copy against the publication hash instead.

## Restore the graph

Use a NEW EMPTY Neo4j target database (source version: Community 5.26.24). Follow knowledge_graph/README.md inside the graph package. Using locally configured cypher-shell authentication, run graph_nodes.cypher, graph_relationships.cypher, graph_finalize.cypher, schema.cypher and verify.cypher in that order. No APOC or model service is required. Do not run the restoration statements on your existing source database.

JSONL records retain labels, all exported properties and original business identifiers. Tagged temporal values preserve native types. Snapshot export IDs connect relationships to nodes; they are not promised to equal element IDs assigned by a reconstructed database. Two original KnowledgeBaseBuild metadata records are preserved; the one matching the evidence store has artifact_set_hash `f4f24447b00405584ad4da2ad1cf3e1a847751421edfa15f78c7b27ba50e002b`.

## Validation and scope

All 306 document IDs, 58,169 clause IDs/FROM_DOCUMENT mappings and 33,207 SUPPORTED_BY link keys match the SQLite evidence store. Export counts/endpoints, SQLite integrity/foreign keys/FTS, archive CRCs and representative Cypher syntax were checked. A complete independent graph restoration was not performed during packaging; consult the manifest for this limitation.

These assets contain research data, Cypher data-restoration statements and documentation. API service implementations, credentials, connection configuration, runtime logs and model-call caches are excluded. The source graph and evidence database were not modified.

## English source-directory names

All repository directory names use English. Original document filenames and file contents are unchanged. The complete old-to-new directory mapping is in [folder_name_mapping.json](folder_name_mapping.json).

The published `data-v1.0.0` archives are frozen snapshots, so their embedded `document.file_path` values and `documents.csv` retain the original directory paths. Use the repository's current `documents.csv`, joined by `document_id` or `original_relative_path`, to locate each file at its new `relative_path`. No database changes are needed. Archive member hashes in `manifest.json` describe the frozen archive files, not the updated repository document catalog.
