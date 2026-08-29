# Customer360 KT Master Prompt

Act as a senior developer giving me a detailed Knowledge Transfer (KT) based on the **actual Customer360 repository**.

## Rules
- Inspect the repository broadly before writing.
- Base claims on actual code; do not infer behavior from filenames alone.
- Trace imports, function calls, configuration references, shell commands, XML properties, paths, and dependencies.
- Always give exact repository-relative paths.
- Distinguish **Repository fact**, **Inference**, and **[NEEDS TEAM CONFIRMATION]**.
- Never invent queues, service accounts, keytabs, HDFS paths, CA7 behavior, or other infrastructure.
- Explain WHY components exist and HOW files connect.
- Use Mermaid diagrams where useful.
- Use `new_scorecard/pyspark/src/jobs/amg_client_facts` as the primary example.
- Assume I know Python and SQL but am newer to enterprise PySpark/Hadoop/Oozie.

## Primary deliverable
Do not only answer in chat. Create/update these formatted Markdown KT documents under `docs/kt/`:

1. `00_KT_INDEX.md`
2. `01_REPOSITORY_MAP.md`
3. `02_EXECUTION_FLOW.md`
4. `03_MAIN_PY_DEEP_DIVE.md`
5. `04_AMG_CLIENT_FACTS_DEEP_DIVE.md`
6. `05_CONFIGURATION_SYSTEM.md`
7. `06_AAI_JOBS_ZIP_AND_SPARK_SUBMIT.md`
8. `07_SHELL_SCRIPTS.md`
9. `08_OOZIE_WORKFLOW.md`
10. `09_DEPLOYMENT_FLOW.md`
11. `10_MANUAL_PYSPARK_QA_RUNBOOK.md`
12. `11_FULL_OOZIE_RUNBOOK.md`
13. `12_TROUBLESHOOTING.md`
14. `13_LEARNING_ROADMAP.md`
15. `14_OPEN_QUESTIONS_FOR_TEAM.md`

`00_KT_INDEX.md` must link to every document and provide a recommended reading order.

## 1 — Repository map
Create a simplified important directory tree. Focus on `__main__.py`, `__init__.py`, PySpark source/jobs, configuration, scripts, Makefiles/build files, Oozie XML/properties, environment configuration, deployment/post-deployment code, `new_scorecard`, and shared libraries.

For each important component explain its purpose, consumer, dependencies, and whether it is source/config/deployment/runtime code.

## 2 — Actual execution flow
Trace `new_scorecard/pyspark/src/jobs/amg_client_facts` from launch through Spark submission, `__main__.py`, arguments, configuration, job discovery/import, Spark creation, `run(...)`, transformations, and output. Name the exact file responsible for every step. Add Mermaid diagrams.

## 3 — `__main__.py`
Walk through it section-by-section. Explain `--job`, `--env`, `--job-args`, ZIP/job discovery, configuration discovery, `importlib`, job-to-module mapping, product determination, Spark configuration, and invocation of `run()`.

Explicitly trace:

`spark3-submit → __main__.py → --job amg_client_facts → module import → run(...)`

Use short code excerpts only.

## 4 — `amg_client_facts`
Analyze `new_scorecard/pyspark/src/jobs/amg_client_facts/__init__.py`.

Explain imports, `run()` parameters, `run_date`, inputs, transformations, joins, filters, business logic, outputs, Spark actions, logging, and configuration dependencies. Organize by logical stages.

Include `INPUTS → TRANSFORMATIONS → OUTPUTS` and a Mermaid diagram.

## 5 — Configuration
Trace shared, job-specific, environment-specific, and override configuration. Determine actual precedence from code. Explain how `--env qa` affects execution. Mention LAB/PROD only when present in code.

## 6 — `aai-jobs.zip` and Spark submit
Determine how the ZIP is built, its expected tree, included/excluded files, role of `__main__.py`, and how `--py-files` works here.

Find a real Spark submit command/script and explain `spark3-submit`, `--master`, `--queue`, `--deploy-mode`, `--executor-cores`, `--conf`, `--jars`, `--files`, `--py-files`, entry point, `--job`, `--env`, and `--job-args`.

Construct an `amg_client_facts` example but mark unknown infrastructure `[NEEDS TEAM CONFIRMATION]`.

## 7 — Shell scripts
Identify important scripts for Spark submission, deployment, post-deployment, copying, environment setup, logging, and Oozie. For major scripts show:

`WHO CALLS IT → WHAT IT DOES → WHAT IT CALLS NEXT`

Deep-dive 2–4 important scripts.

## 8 — Oozie
Find real workflow XML. Explain its workflow-app/start/actions/Spark or shell actions/ok/error/kill/properties/HDFS references. Create a Mermaid workflow diagram.

## 9 — Deployment
Trace actual repository evidence for:

`Git/source → build/package → aai-jobs.zip → Edge Node/local filesystem → HDFS → Oozie → Spark/YARN`

Clearly distinguish Edge Node, HDFS, Spark/YARN, Oozie, and CA7. If an external step is not represented in Git, write:

`[NEEDS TEAM CONFIRMATION] Not determinable from repository alone.`

## 10 — Manual QA PySpark runbook
Create a step-by-step runbook for manually running `amg_client_facts` starting with `aai-jobs.zip` on the QA Edge Node. Cover authentication, files, paths, Spark command, `--job amg_client_facts`, `--env qa`, `run_date`, logs, YARN application ID, and success/failure checks.

Never invent infrastructure. For each missing value provide the exact question to ask the team.

## 11 — Full Oozie runbook
Create a separate step-by-step full workflow tutorial. Explicitly compare **manual individual PySpark execution** versus **full Oozie workflow execution**, including when/why HDFS is required.

## 12 — Troubleshooting
Cover job-not-found, import errors, bad ZIP structure, missing `__main__.py`, missing HDFS files, Kerberos expiration, YARN failure, executor failure, missing JAR, wrong environment config, Oozie failure, and shell failure. Explain where to look first.

## 13 — Learning roadmap
Give me an ordered study path based on the actual architecture so I do not randomly read hundreds of files.

## 14 — Open team questions
Collect every unresolved infrastructure/organizational issue. For each record:
- what is unknown
- why Git cannot answer it
- why it matters
- exact teammate question
- which KT document depends on it

## Final behavior
If later investigation corrects an earlier inference, update the affected Markdown document rather than leaving contradictions.

After creating/updating the files, reply in chat only with:
1. files created/updated,
2. three most important architectural findings,
3. unresolved questions blocking an actual QA/Oozie run.
