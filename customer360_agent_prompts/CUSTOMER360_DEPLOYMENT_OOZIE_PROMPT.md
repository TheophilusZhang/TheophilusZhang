# Customer360 Deployment + Oozie Deep-Dive Prompt

Read the existing `docs/kt/` documentation first. Treat source code as authoritative and update stale documentation.

Trace actual repository evidence for:

`source → build/package → aai-jobs.zip → Edge Node/local filesystem → HDFS → Oozie → Spark/YARN`

For every arrow identify the actual Makefile/build target, shell/post-deploy script, properties file, XML workflow, configuration file, or command.

Explicitly distinguish:
1. manual execution of one PySpark job,
2. artifact deployment,
3. copy/synchronization into HDFS,
4. Oozie workflow execution,
5. external scheduling such as CA7.

If an external step cannot be determined from Git, write:

`[NEEDS TEAM CONFIRMATION] Not determinable from repository alone.`

Never invent paths, accounts, queues, keytabs, or commands.

Create/update:
- `docs/kt/09_DEPLOYMENT_FLOW.md`
- `docs/kt/10_MANUAL_PYSPARK_QA_RUNBOOK.md`
- `docs/kt/11_FULL_OOZIE_RUNBOOK.md`
- `docs/kt/14_OPEN_QUESTIONS_FOR_TEAM.md`

Use Mermaid diagrams. Use `amg_client_facts` as the manual QA example. Unknown executable values must use placeholders such as `<QA_QUEUE>` plus `[NEEDS TEAM CONFIRMATION]`.

Do not merely answer in chat; update the Markdown files.
