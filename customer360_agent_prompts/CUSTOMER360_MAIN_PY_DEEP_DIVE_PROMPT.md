# Customer360 `__main__.py` Deep-Dive Prompt

Read `docs/kt/00_KT_INDEX.md`, `docs/kt/02_EXECUTION_FLOW.md`, and `docs/kt/03_MAIN_PY_DEEP_DIVE.md` first if they exist. Treat source code as authoritative.

Focus only on the repository's actual `__main__.py`.

Trace:

`spark3-submit`
→ `__main__.py`
→ argument parsing
→ `--job amg_client_facts`
→ job discovery in package/ZIP
→ owning product/project
→ dynamic module import
→ shared/job/environment configuration
→ Spark creation/configuration
→ `run_date`
→ `run(spark, logger, run_date)`

Requirements:
- Detailed code walkthrough, not summary.
- Exact repository paths.
- Short code excerpts.
- Explain dynamic imports and important variables.
- Distinguish facts/inferences.
- Mark environment unknowns `[NEEDS TEAM CONFIRMATION]`.
- Include Mermaid diagrams.
- Correct other KT documents if new evidence changes earlier conclusions.

Primary output: `docs/kt/03_MAIN_PY_DEEP_DIVE.md`.

Also update `docs/kt/02_EXECUTION_FLOW.md` and `docs/kt/14_OPEN_QUESTIONS_FOR_TEAM.md` when needed.

Do not merely answer in chat; update the Markdown files.
