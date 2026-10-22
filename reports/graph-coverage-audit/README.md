# Graph coverage audit — ReVeal/test

Prepare completed: 9819/9819 graph jobs; 3070 newly built, 6749 reused.
Test coverage: 2163/2274 (95.1187%); 111 unavailable graphs.
All required graphs, including retrieval candidates: 805/9819 have no nodes or edges.
All 111 unavailable test graphs have status joern_no_function_ast and empty nodes/edges;
50 use joern_whole_graph_v3 and 61 use grace_h1_graph_v1.
100/111 exported only 18 nodes and 42 edges before function ownership filtering.

## Source inspection (categories are diagnostic hypotheses)

- 97 missing macro context: 92 `_U_`, 1 `ATTRIBUTE_UNUSED`, 1 `HB_UNUSED`, 2 `TSRMLS_DC`, 1 `ATTR_UNUSED`.
- 10 contain `##` token pasting / macro fragments.
- 2 C++ snippets with tokenized `: :`.
- 2 are not standalone function definitions (assertion fragment and global declaration/macro fragment).

## Reproduction using installed Joern and existing parse_graphml

- Original `_U_` sample: 0 nodes / 0 edges; diagnostic prefix `#define _U_`: 31 / 241.
- ATTRIBUTE_UNUSED sample with diagnostic prefix definition: 14 / 76.
- Original C++ sample: 0 / 0; restoring `::` alone in `.c`: 0 / 0;
  `.cpp` alone: 0 / 0; restoring `::` plus `.cpp`: 26 / 160.
- Empty --define NAME= failed in installed frontend; do not adopt that syntax without validation.
- Diagnostic prefixes and token restoration alter parser input; production code/cache was not changed.
- Representative experiments do not establish recovery rate across all affected samples.

## Recommended repair

1. Preserve the current frozen run as baseline. Retry unavailable graphs only in a separate versioned cache.
2. Preserve original code and code hash; record parser-input hash, macro profile, language/extension,
   Joern version, parser flags, transformation version and logs as graph provenance/cache identity.
3. For annotation macros, supply verified project definitions through parser context/header.
   Test each macro family; do not blindly erase semantic macros (notably TSRMLS variants).
4. For C++ use `.cpp` plus a token-aware repair of split operators; never globally replace text
   inside strings/comments. Keep original code for prompts and record graph derivation.
5. Macro token pasting requires original macro definitions/arguments and preferably build context
   or compilation database. Do not invent expansions or wrap arbitrary fragments as fake functions.
6. Keep truly unparseable snippets explicitly unavailable; report coverage and code-only behavior.
7. Audit all 805 unavailable required graphs (targets and retrieval candidates), then regenerate
   dependent graph indexes, reranking, prompts and freeze manifests in a new run.
8. Before inference, compare coverage, parse diagnostics and representative graph contents,
   keeping the evaluation cohort unchanged. More nodes alone do not prove a correct graph.

Current GraphStore.ensure reuses an existing empty graph without retry; rerunning prepare alone
will not fix these entries. Existing frozen preparation can also bypass rebuilding entirely.

See affected-test-samples.json for all 111 source snippets and experiments*.json for measured results.
