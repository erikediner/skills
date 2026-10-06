---
name: codebase-overview
description: Onboards the developer to an unfamiliar codebase and generates or updates the README.
disable-model-invocation: true
---

# codebase-overview

You are an experienced developer who maps an unfamiliar codebase and writes clear documentation.

## Step 1: Map the codebase

Start with manifest files (`package.json`, `*.csproj`, `pom.xml`, `requirements.txt` and so on),
the root README and the folder structure to get an overview. Follow threads that look
interesting or unusual, and dig deeper when something does not make sense yet.

Find:

- **Language and framework:** primary language, versions, build system
- **Architecture:** monolith, microservices, layered, modular and so on
- **Key terms:** domain words, pattern names, abbreviations used internally
- **Patterns:** repository pattern, event-driven, CQRS, DI and so on
- **Unusual setup:** deviations from conventions, odd configuration, outdated dependencies

> If you notice something security related in passing (a hardcoded secret, an
> obviously outdated package): note it as a keyword, but do not dig into it here.
> The full review (STRIDE, OWASP, privacy, dependencies) is the job of
> `security-analysis`, which runs after this one.

Always check:
- Existing `README.md` files (root and modules)
- Configuration files (`.env*`, `docker-compose*`, CI files)
- Dependency files (`package.json`, `requirements.txt`, `*.csproj` and so on)

## Step 2: Clarify open questions with the user

After mapping: ask short questions about things the codebase did not reveal.

Typical examples:
- Domain: "What is the difference between [TermA] and [TermB] in this context?"
- Architecture intent: "Why is [module X] split out from [module Y]?"
- Unused code: "It looks like [file/module] is not in use. Is it active or can it be ignored?"
- Environments: "Is there staging or prod config somewhere other than the repo?"

Rules:
- At most 4 questions at a time, grouped as a list
- Suggest an answer where it is natural
- Wait for answers before you go on to step 3 (draft)

## Step 3: Show a draft in the chat

Write a first draft of the README directly in the chat based on the template in
[README-TEMPLATE.md](README-TEMPLATE.md). Do not save any file yet.

- Write the README in Norwegian. Keep code, commands and identifiers as they are
- Fill in every section you have coverage for
- Mark sections with `<!-- TODO -->` where information is missing. Do not guess
- Put today's date in `<!-- Generert av KI med menneskelig supervisjon. Sist oppdatert: [DATO] -->`

## Step 4: Ask about placement and splitting

Based on what you found, recommend one of these and ask for confirmation:

- a) One README.md at the root
- b) One README.md per module, linked from the root README
- c) Both

> Example: "The codebase is a monolith with three clear modules. I recommend (b). OK?"

If a README.md already exists: ask whether to **update it** or **create a new file** (for example `OVERSIKT.md`).

## Step 5: Save

Save the draft where the user chose. If split per module: create a root README with
links to each module README.

## Step 6: Summarize findings

After the README is written, give a short summary (5 to 10 bullet points) of:
- The most important things to know about the codebase
- Unusual setup that may surprise new developers
- Technical debt that should be addressed soon

If you caught security keywords in step 1: mention them in one line and recommend
`security-analysis` as the next step. Do not prioritize or expand on them here.

## Step 7: Write key facts to repo memory

Save core facts in `/memories/repo/codebase.md` so that later skills
(especially `grill-spec`) do not have to repeat the exploration. Keep it short, at most
about 20 lines. Include:

- Primary language and version, main framework
- Architecture pattern (one sentence)
- Build and test commands
- 3 to 5 central domain terms
- 1 or 2 things that surprise new developers

If the file already exists: update it instead of duplicating. Put
`Last updated: [DATE]` at the top.