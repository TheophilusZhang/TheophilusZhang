# How to Manage the Customer360 VS Code Agent with Markdown

## Suggested layout

```text
customer360/
├── .agent/
│   └── prompts/
│       ├── CUSTOMER360_KT_MASTER_PROMPT.md
│       ├── CUSTOMER360_MAIN_PY_DEEP_DIVE_PROMPT.md
│       └── CUSTOMER360_DEPLOYMENT_OOZIE_PROMPT.md
└── docs/
    └── kt/
```

Keep **agent instructions** in `.agent/prompts/` and **agent-generated KT documentation** in `docs/kt/`.

## Step 1 — Copy these prompt files into the repo

Put the three prompt Markdown files under `.agent/prompts/`.

Create `docs/kt/` or let the agent create it.

## Step 2 — Open the repository root

Open the entire `customer360` repository folder in VS Code, not only `new_scorecard`. Repository-wide access is needed to trace build scripts, configuration, Oozie, and shared code.

## Step 3 — Run the master investigation

Open `.agent/prompts/CUSTOMER360_KT_MASTER_PROMPT.md`.

Tell the agent:

> Follow `.agent/prompts/CUSTOMER360_KT_MASTER_PROMPT.md`. Inspect the repository first and create the requested Markdown KT documents under `docs/kt/`. You may inspect the whole repository, but do not modify application source code.

If your agent supports file references/attachments, reference the prompt file instead of pasting its contents.

## Step 4 — Read the KT as formatted Markdown

Start with `docs/kt/00_KT_INDEX.md`.

VS Code Markdown Preview:
- Windows/Linux: `Ctrl+Shift+V`
- macOS: `Cmd+Shift+V`

Or use Command Palette → `Markdown: Open Preview to the Side`.

## Step 5 — Run focused follow-up agents

After the initial KT, run the `__main__.py` prompt, then the deployment/Oozie prompt.

Tell the agent to **update the existing docs**, not create a disconnected chat explanation.

## Step 6 — Use the docs as durable context

At the beginning of later sessions, use:

> Read `docs/kt/00_KT_INDEX.md` and the KT documents relevant to this task before inspecting source code. Treat source code as authoritative. If the documentation is stale, update it.

Useful follow-ups:

> Read `docs/kt/02_EXECUTION_FLOW.md` and the source files it cites. Teach me only the path from `--job amg_client_facts` to `run()`.

> Verify `docs/kt/06_AAI_JOBS_ZIP_AND_SPARK_SUBMIT.md` against current source and correct inaccuracies.

> Read `docs/kt/14_OPEN_QUESTIONS_FOR_TEAM.md` and turn the unresolved QA items into concise questions for my lead.

## Step 7 — Protect application code during learning sessions

Use this instruction:

> You may inspect the entire repository but modify only `.agent/prompts/` and `docs/kt/`. Do not modify Python, SQL, configuration, shell scripts, XML, Makefiles, or other application/deployment files unless I explicitly ask.

Review `git diff` after an agent session.

## Step 8 — Version the KT with Git

Because the KT is Markdown inside the repository, Git can show what the agent learned or changed over time.

Typical checks:

```bash
git status
git diff -- docs/kt
```

Commit the documentation only if your team's repository policy allows it. Otherwise keep `docs/kt/` local or add it to your personal exclude configuration rather than changing the shared `.gitignore`.

## Important: Markdown prompts vs persistent agent instructions

A Markdown file does not automatically control every VS Code agent. The portable method is to explicitly tell the agent to read/follow the prompt file.

Some VS Code agent products support dedicated repository instruction/custom-agent files. If yours does, you can later put a **small permanent rule set** there (for example: inspect source before answering; do not modify application code during KT; use `docs/kt/`), while keeping these long task prompts in `.agent/prompts/`.

This separation is useful:

```text
Permanent agent rules      → short, always applicable
.agent/prompts/*.md        → reusable investigation tasks
docs/kt/*.md               → accumulated repository knowledge
Source code                → authoritative truth
```
