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
