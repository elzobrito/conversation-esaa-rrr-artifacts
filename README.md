# RRR Memory Consolidation: Synthetic Evaluation Artifacts

This repository contains a frozen source snapshot, synthetic fixtures and results for the revision experiment. It contains no conversation ledger or private case study. The source is a working-tree snapshot of Conversation ESAA, not a claim that the same version is installed or released. The software license is MIT; see LICENSE.

## Reproduce

Requires Node.js 18+, Python 3 and `tiktoken==0.12.0`. First access to `cl100k_base` may download its vocabulary.

```sh
python -m venv .venv
.venv/bin/pip install -r tests/memory/requirements-evaluation.txt
.venv/bin/python tests/memory/revision-evaluation.py /tmp/rrr-results.json
python tests/memory/test-revision-result.py
node tests/memory/adversarial-evaluation.js /tmp/rrr-adversarial.json
```

Compare all `runs` fields except `elapsed_ms` and `max_rss_kib`, which are environment-dependent. Seeds change sampling, not the synthetic fixture. Token accounting is the sum of separately encoded canonical JSON records, not API chat-token usage. The oracle is a decision-only cardinality ceiling with empty evidence, conditional on the representation; it is not a ceiling for conflicts or provenance. CN reuses the production context order with standardized JSON, not production Markdown rendering. B is a deliberately simple local recency baseline, not Mem0. The six adversarial pairs are hand-authored and not independent of implementation development. No results support claims of universal superiority or subjective dreaming.

## Version 1.2: authority-aware baseline

B′ chooses the latest explicitly authoritative decision for each topic/key, ignores episode echoes, and uses the same synthetic structured keys and evidence as the other arms. It is not Mem0. The updated report includes conditional provenance and conflict precision/recall.

```sh
.venv/bin/python tests/memory/v12-evaluation.py /tmp/rrr-v12.json
python tests/memory/test-v12-result.py
node --test tests/memory/authority-baseline.test.js
```

## Version 1.3: conflict quota and pre-cut counts

The v1.3 report stores pre-cut true/false positive counts and adds CQ, a 20% token quota for conflicts applied to C's candidate list in proposal (`conflict_id`) order. CQ does not rerun consolidation. The implementation does not export a per-conflict confidence score.

```sh
.venv/bin/python tests/memory/v13-evaluation.py /tmp/rrr-v13.json
python tests/memory/test-v13-result.py
node --test tests/memory/authority-baseline.test.js
```

The checked-in `docs/qa/CONV-RRR-V13-OUTPUT.json` is the report cited by the article (schema `conv-rrr.v13.v1`, 405 runs). Compare deterministic fields except `elapsed_ms` and `max_rss_kib`. Experiments are synthetic, not independently replicated, and do not test general superiority of selectionist over instructionist memory management.
