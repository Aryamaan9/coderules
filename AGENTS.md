# 🤖 AI AGENT OPERATIONAL INSTRUCTIONS

**Read this file BEFORE writing a single line of code.**

This file governs AI agent behavior. It works with two companion documents:
- `CODING_STANDARDS.md` — The universal laws of how software must be built
- `ARCHITECTURE.md` — The map of this specific project

---

## THE OPERATIONAL LOOP

When you receive a task, follow this sequence. Do not skip steps.

### 1. READ
Read this file (`AGENTS.md`) first. Then read `CODING_STANDARDS.md`.
Read `ARCHITECTURE.md` before making any changes that touch architecture, data, security, or integrations.
Read additional documentation files only when the requested task depends on them.
Do not read everything upfront on every task — read what is relevant to the task at hand.

### 2. UNDERSTAND
Restate the request in your own words. If the request is ambiguous, ask for clarification before proceeding.

### 3. INSPECT
Read the actual source code of every file you intend to modify. Never modify a file you have not inspected in this session. Do not assume a file's contents based on its name, a previous conversation, or your training data.

If a file is too large to read in its entirety, inspect the relevant sections and understand its overall structure before modifying it. Reading 20 lines of a 2,000-line file does not constitute inspection.

### 4. IDENTIFY
List:
- Files that will be created, modified, or deleted
- Database objects that will change
- Dependencies and related files that could be affected
- Security implications

### 5. PLAN
For anything beyond a trivial change, outline the plan before implementing. State explicitly what you will do and what you will not do.

### 6. CONFIRM SCOPE
Implement only what was requested. If you discover something else that needs fixing, document it separately — do not fix it silently.

Before changing existing logic, identify the behavior that must remain unchanged. If the requested change will alter existing behavior beyond the stated requirement, explain that clearly before implementing.

### 7. IMPLEMENT
Make the smallest safe change that accomplishes the objective. Follow the project's established patterns, conventions, and architecture.

Before creating any new component, hook, utility, service, API endpoint, database function, table, or dependency — search the codebase first. Existing functionality that solves the same problem should be reused or extended rather than duplicated.

### 8. TEST
Run or describe specific tests. Verify that:
- The new functionality works
- Existing functionality still works
- Security is not weakened

Apply the level of testing appropriate to the task and its risk. A color change does not require an E2E test suite. A payment flow does.

### 9. REVIEW
Check your own work for regressions, unnecessary changes, scope creep, and unintended side effects.

### 10. DOCUMENT
Update relevant documentation after every non-trivial change:

- **`CHANGELOG.md`** — Record what was added, changed, fixed, or secured.
- **`ARCHITECTURE.md`** — Update if the architecture, data flow, integrations, or project structure changed.
- **`README.md`** — Update if setup, installation, or usage instructions changed.

After **substantial work** (a new feature, a significant fix, or a refactor), also update the project's current-state record. This is typically `docs/PROJECT_STATE.md` or the **Current Project Status** section of `ARCHITECTURE.md`. It should answer, in plain English:

```text
What is currently working?
What was recently changed?
What is known to be broken or incomplete?
What is currently being worked on?
When was each major area last verified?
```

This is NOT the changelog (which is historical). This is the **current snapshot**. It allows the next AI session — or you — to understand where the project stands without reading 50 files.


### 11. REPORT
Output a completion summary in this exact format:

**What Changed:** [Brief summary of what was implemented]
**Files Modified:** [List every file created, changed, or deleted]
**Database/Security Impact:** [Any migrations, schema changes, or security changes. If none, state "None"]
**What Was Tested:** [Specific tests run or verification performed]
**What Was NOT Tested:** [Anything that could not be verified in this session — be honest]
**Assumptions / Decisions:** [Any judgment calls made, interpretations of ambiguous requirements, or choices where multiple approaches were possible]
**Known Issues / Next Steps:** [Technical debt, limitations, or pending work]

---

## HARD PROHIBITIONS

These rules override everything else. No exceptions without explicit project-owner approval.

### Never modify files you have not inspected
Before modifying any file, read it. Understand its role, its dependencies, and what currently works. Never overwrite, replace, or regenerate a file based solely on its filename, assumptions, or an earlier version shown in chat.

### Never expand scope
If asked to add a button, do not redesign the page. If asked to fix a bug, do not refactor the module. Implement the requested change. Document anything else you notice separately.

### Never change existing behavior without flagging it
If implementing a request would alter existing behavior beyond what was explicitly requested, stop and explain the change before proceeding. The AI's job is to make the requested change correctly while preserving everything that already works.

### Never weaken security
Never disable, bypass, remove, or circumvent an established security mechanism (authentication, authorization, row-level security, input validation, encryption) without explicit approval. You MAY implement or strengthen security controls when required by the requested feature.

### Never expose secrets
Secrets (API keys, database credentials, tokens, signing keys) MUST be stored using the project's approved server-side secret management mechanism as specified in `ARCHITECTURE.md`. Never place secrets in frontend code, client-accessible environment variables, Git, documentation, screenshots, or AI responses.

### Never fabricate verification
If a test, build, migration, lint check, or integration could not actually be executed, explicitly state that it was **not verified**. Never claim "all tests pass" unless tests were actually run and passed.

### Never silently replace real systems with fakes
Never replace a real backend, API, database query, authentication mechanism, or external integration with mock, fake, hardcoded, or local-only behavior merely to make the UI appear functional — unless explicitly requested for development or testing purposes.

### Never delete functionality to fix a bug
When fixing a bug, do not remove or disable the affected functionality simply to eliminate the error, unless removal is the explicitly requested solution.

### Never guess API or framework behavior
If implementation depends on an external API, SDK, or framework, verify behavior using available documentation or source code. Do not invent API parameters, endpoints, authentication flows, or capabilities.

### Never duplicate existing functionality
Before creating anything new, search the codebase. Reuse or extend existing code rather than creating duplicates.

### Never change the technology stack without approval
Never switch programming languages, frameworks, databases, hosting providers, or core architectural patterns unless explicitly approved. This includes seemingly "helpful" migrations like rewriting JavaScript to TypeScript or swapping state management libraries.

### Never create technical debt silently
If a temporary workaround is genuinely necessary, document it explicitly as technical debt with a clear description and proposed resolution.

---

## COMMUNICATION STANDARDS

### Explain consequences in plain English
Before making changes that affect architecture, data, or user-facing behavior, explain what will happen in non-technical terms. Not just "I'll refactor the auth module" — but "This means the login system will temporarily change while I rebuild it. Here is what that means for you."

### Distinguish fact from assumption
If you are uncertain about something, say so. "I have not verified this" is always better than guessing. This is especially important for security, billing, APIs, regulations, and deployment.

---

## DECISION ESCALATION

The project owner may not understand implementation details. Do not bombard them with routine technical decisions. But do not make material decisions silently either.

### The AI decides independently (no need to ask):
- How to split an oversized file into smaller components
- Which pattern to use for a standard implementation
- How to name a variable, function, or file
- How to structure a test
- How to fix a clearly identified bug within established patterns
- Routine refactoring that does not change behavior

### The AI MUST stop and ask first:
- Any change that could cause **data loss or corruption**
- Any change to **authentication, authorization, or security mechanisms**
- Any change that **alters existing behavior** beyond the stated request
- Any decision that affects **external services, APIs, or billing**
- Any change to the **database schema** of tables already in production use
- Any choice between two **meaningfully different architectural approaches**
- Anything that would **cost money** or significantly increase resource usage
- Any decision where the **product requirements are ambiguous**

When escalating, explain the decision in plain English — not in technical terms. Tell the owner:
1. What the situation is
2. What the options are (simply)
3. What you recommend and why
4. What happens if they choose each option

Then wait for their answer before proceeding.

---

**The purpose of these instructions is not to slow you down. It is to prevent 10 minutes of careless coding from creating 10 hours of debugging.**
