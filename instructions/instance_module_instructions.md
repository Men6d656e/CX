# Instance Module — Instructions

## Purpose
The **instance module** is the single word that names ALL the ways the user can
interact with agents and the context system. It covers the chat-history module,
the tasks-history module, AND the agent's own prompt interface. When any LLM
hears "instance module" it knows the complete set of input methods — it does
NOT mean only one of the three instances.

## Hard Rule — This Is Not Optional

This section is a hard lock. Any agent that reads this file **must** obey it.
No deviation, no shortcut, no guessing. If a rule below says "must", it means
there is no other allowed path. The system was designed with exactly 3 instances;
if none of them fit, the rule says to stop and ask — not to invent a 4th route.

### The 3-instance lock

Every user input that reaches an agent MUST resolve to exactly one of these three
instances. There is no 4th instance. There is no "just answer from memory" path.
There is no bypassing the instance module.

| If the user... | Then the instance is... | And the agent must... |
|---|---|---|
| points at a Q file or says "see Q(N).txt" or "address the questions defined using the 1st instance" | **1st** — chat-history module | follow `chat_history_instructions.md` exactly |
| points at a T file or says "see T(NNN).txt" or "address the tasks defined using the 2nd instance" | **2nd** — tasks-history module | follow `tasks_history_instructions.md` exactly |
| types a raw prompt into the agent, with or without `@SYSTEM.md`, and does NOT point at a Q or T file | **3rd** — agent's own prompt interface | follow this file's 3rd-instance workflow exactly |

### How the agent chooses the instance (mandatory decision flow)

When the user gives any input, the agent must run this decision in order. Stop at
the first match. Do not skip steps. Do not "look at the prompt size" and pick a
different instance.

0. **Guide trigger check — BEFORE everything else.** Scan the prompt for an
   action word (build / make / create / compile / write / draft / produce / save /
   generate / prepare) + the word "guide". If found, the guide trigger has fired:
   a guide MUST be built in `guides/` this turn (see
   `guides_module_instructions.md`), in addition to — never instead of — whatever
   instance the prompt resolves to below. This check is never skipped.
   **Utility trigger check — also here.** If the prompt says "use the utility
   <name>", resolve and run it per `utility_module_instructions.md`, inside the
   instance resolved below.
1. **Is there an explicit Q/N or "1st instance" reference in the prompt?**
   Yes → 1st instance. Read the Q file named. Do not answer from memory.
2. **Is there an explicit T/NNN or "2nd instance" reference in the prompt?**
   Yes → 2nd instance. Read the T file named. Do not do the task from memory.
3. **Is there a file reference at all (Q, T, guide, index, anything)?**
   Yes → That reference determines the instance. Before reading it, verify it
exists. If the referenced file does **not** exist, stop now: reply with only the
error, and create nothing in the context engine. If it does exist, read it;
the file's location and type tell you which instance you are in. Do not
override it because the prompt "looks short" or "looks long".
4. **Otherwise — no file is mentioned, or the only file reference is `@SYSTEM.md`**
   (which is the engine bootstrap, not a task file) → **3rd instance**.
   Create the T file yourself. Do not ask the user to make one. Do not answer
   the prompt inline without creating the tracking files.

### The short-prompt trap — explicit prohibition

A common failure mode: a short prompt like *"follow @SYSTEM.md and tell me the
changes last night"* is treated as a casual question and answered inline.
**This is wrong.** That prompt contains no Q file and no T file, so by the decision
flow above it is the **3rd instance**. The agent **must** create the T file and
all the tracking files. A prompt being short does not make it a 1st-instance
question, and it does not make it a free-form chat. The only thing that decides
the instance is whether the user pointed at a Q or T file.

Similarly: a long, detailed prompt does not "automatically" become the 3rd
instance out of leniency — it is the 3rd instance because no Q/T file was named.
The same rule applies to both. Length is not a signal.

### What "follow @SYSTEM.md" means in every instance

`@SYSTEM.md` is the bootstrap cue. It means: read SYSTEM.md, then read INDEX.md,
then read the instruction file for the module you are about to touch. It does NOT
mean "skip the instance decision" or "answer however you like". After bootstrap,
the agent still has to pick an instance using the decision flow above.

### The phrase "address the {problem,task,question} below" — what it really means

When the user writes something like *"follow @SYSTEM.md and address the problem
below"* or *"follow @SYSTEM.md and address this task"*, that is a 3rd-instance
prompt. The agent:
1. writes the user's prompt word-for-word into `tasks-history/definitions/T(NNN).txt`,
2. creates `active/T(NNN)-tracker.md` and `logs/T(NNN)-log.md`,
3. does the work,
4. writes `executions/E(NNN).md`,
5. appends rows to `tasks-history/INDEX.md` with **Instance = 3**,
6. reports the execution file path.

The user did not create a T file, so the agent creates one. That is the whole
point of the 3rd instance. The agent must NOT answer inline and then say "done".

### No inline-only answers

There is no mode in this system where the agent answers a prompt inline and treats
that as the final result. Even a one-line question is either:
- a 1st-instance question (if it points at a Q file), or
- a 3rd-instance task (if it does not).

In the 1st instance, the answer goes to `chat-history/outputs/A(N).md`.
In the 3rd instance, the task goes through `tasks-history/` and the result goes
to `executions/E(NNN).md`.

The only exception is when the user explicitly asks for a quick, throwaway
clarification **inside an already-open task**. That is not a new prompt — it is
part of an existing T file's work, and it belongs in the log for that task.

### No guessing the user's intent about the instance

If the user's prompt is ambiguous, the agent does **not** pick the instance that
seems easiest. Ambiguity is resolved by asking. Specifically:
- If the user mentions a file name but it is unclear which module it belongs to,
  read the file first; the file's location tells you the instance.
- If the user says something that could be read as either a question or a task,
  treat it as a 3rd-instance task (the more structured path) unless the user
  clearly said otherwise.
- If you genuinely cannot tell, ask the user: "Do you want this as a 1st-instance
  Q file, a 2nd-instance T file, or as a 3rd-instance task?" Then wait.

### No silent fallback

An agent must never silently fall back to a different instance because the right
one "seems like more work". If the prompt is a 3rd-instance prompt, do the full
3rd-instance workflow. If it is short, do the full 3rd-instance workflow anyway.
If it is long, do the full 3rd-instance workflow anyway.

### Cross-instance contamination

- Do not answer a 3rd-instance prompt by creating chat-history files. That is the
  1st instance. The 3rd instance uses tasks-history.
- Do not answer a 1st-instance prompt by creating T/E files. That is the 2nd/3rd
  instance. The 1st instance uses chat-history.
- Do not mix the two to "save a step". The instance is decided by the user's
  input, not by the agent's convenience.

### Media — the user places it, not the agent

Images are placed by the user, into the right media directory, **before** the
prompt is sent. The agent does not hunt for images, does not accept pasted images
as the primary storage path, and does not decide where an image lives.

- 1st instance → the user puts the image in `chat-history/media/`, then names it
  by filename inside the Q file (or inside the prompt that references the Q file).
- 2nd instance → the user puts the image in `tasks-history/media/`, then names it
  by filename inside the T file (or inside the prompt that references the T file).
- 3rd instance → the user puts the image in `tasks-history/media/` first, then
  sends the prompt naming the image by filename along with the problem/task/question.

The agent's job is to read whatever the user already placed and referenced. The
agent does **not** save pasted images into a media directory as the normal path.
If the CLI hands the agent a pasted image as a saveable file, that is only a
fallback for when the user did not pre-place it. The intended path is always:
user places the image, user names the image, agent reads the image.

This keeps the media directories under user control and makes the image part of
the record (the Q file or T file) instead of a hidden attachment the agent
created on the fly.

### The 3rd instance with an image — the forced pattern

When the user wants to use the 3rd instance **and** include an image, the
compulsory pattern is:

1. The user places the image in `tasks-history/media/` and gives it a filename.
2. The user sends the 3rd-instance prompt. The prompt contains:
   - the system cue (`@SYSTEM.md` or equivalent),
   - the image filename,
   - the problem/task/question.
3. The agent reads the prompt, reads the named image from
   `tasks-history/media/`, creates `tasks-history/definitions/T(NNN).txt` that
   contains the prompt word-for-word **and** names the image file, then proceeds
   with the rest of the 3rd-instance workflow (tracker, log, execution,
   INDEX.md row, Instance = 3).

If the user sends a 3rd-instance prompt that references an image filename but that
image file does not exist in `tasks-history/media/`, the agent does **not**
create the T file, does **not** create any tracking files, and does **not**
proceed with the work. The agent replies only with the error: which file was
referenced, which directory it should be in, and that it is missing. Nothing is
written into the context engine.

The 3rd instance does **not** skip the image check or silently continue without
the image because "the rest of the prompt is still answerable". If the user named
an image, that image is part of the task. Missing image = missing input = stop.

### File references — mandatory verification before any work

This rule applies to **every** instance and **every** kind of referenced file:
Q file, T file, media file, index file, guide file, or any other file the user
mentions ("see that file in abc repo", "read X", "use this file", etc.).

Before the agent does any work, creates any file, or writes anything into the
context engine, it must verify that every file the user referenced actually exists
at the path implied by the prompt and the module wiring.

- If the referenced file exists → proceed.
- If the referenced file does **not** exist → stop. Reply only with the error.
  State which file was referenced, what path was expected, and that it was not
  found. Do **not** create any Q/A/T/E/tracker/log/media/index file in response.
  Do **not** invent a replacement file. Do **not** continue as if the file existed.

This is a hard gate. No task starts without the referenced input being present.
No answer is written without the referenced Q file being present. No media is
used without the referenced media file being present.## Error Handling

### Graphify-first file discovery (every instance)

Whatever the instance, target/related files are located with graphify BEFORE
reading from the filesystem:

1. Run `graphify query "<task topic>"` — the graph indexes this engine's Q/A,
   T/E, guide, notes, AND app submodule code files.
2. Read the files the query surfaces. Directory listing / raw grep are fallbacks
   only (graphify unavailable, empty result, or after `graphify update .`).
3. After the work, run `graphify update .` from the repo root root (never inside
   the app submodule) to index the new files.

This is part of every instance workflow — see `graphify_instructions.md`.

### File Existence Verification (MANDATORY — Hard Gate)

**Before any work, create any file, or write anything into the context engine,
the agent MUST verify that every file the user referenced actually exists.**

This is a hard gate. No task starts without the referenced input being present.
No answer is written without the referenced Q file being present. No media is
used without the referenced media file being present.

#### File Existence Check Workflow

1. User references a file (Q, T, guide, index, media, anything) in the prompt
2. Agent extracts ALL referenced files from the prompt
3. Agent checks EACH file in order:
   - If file EXISTS → continue to next file
   - If file DOES NOT EXIST → STOP immediately
4. If ALL files exist → proceed with the task
5. If ANY file is missing → abort

#### If File is Missing — Required Response

- Reply with ONLY the error
- State which file was referenced
- State what path was expected
- State that it was not found
- State that NOTHING was created in the context engine
- Do NOT suggest alternatives
- Do NOT create the missing file (unless the rule explicitly says the agent creates
  that specific file, e.g., 3rd instance creates T file)
- Do NOT invent replacement files
- Do NOT continue as if the file existed

#### Files That Must Be Checked

| Instance | User References | Agent Must Verify |
|----------|-----------------|------------------|
| 1st (chat-history) | Q(N).txt in prompt | Q file exists in chat-history/inputs/ |
| 1st (chat-history) | Q file references media | Media file exists in chat-history/media/ |
| 2nd (tasks-history) | T(NNN).txt in prompt | T file exists in tasks-history/definitions/ |
| 2nd (tasks-history) | T file references media | Media file exists in tasks-history/media/ |
| 3rd (agent prompt) | User pastes image + prompt | Image saved to tasks-history/media/ (fallback only) |
| Any instance | Guide file named | Guide exists in guides/ (root module) |
| Any instance | INDEX.md or other index | Index file exists at expected path |
| Any instance | File in another repo | File exists in that repo (READ-ONLY check) |

### Duplicate Detection (MANDATORY)

**Before creating A(N).md or E(NNN).md, the agent MUST check if the corresponding
file already exists in INDEX.md.**

#### Duplicate Detection Workflow

1. User references a Q file (1st instance) or T file (2nd instance)
2. Agent checks the relevant INDEX.md:
   - For Q file: check if A(N).md already exists in chat-history/INDEX.md
   - For T file: check if E(NNN).md already exists in tasks-history/INDEX.md
3. If BOTH the source file AND its output file exist in INDEX.md:
   → **ABORT**
   → Reply: "This was already addressed in [output file path]. 
     If you want to extend or follow up, create a new file that references the original."
   → Create NOTHING
4. If source file exists but output file does NOT exist in INDEX.md:
   → Proceed normally (this is new, unindexed work)
5. If source file is NOT in INDEX.md at all:
   → Proceed normally (completely new work)

#### Duplicate Detection Rules

| Situation | Action |
|-----------|--------|
| Q file + A file both exist in INDEX.md | **Abort** — inform user it was already addressed |
| T file + E file both exist in INDEX.md | **Abort** — inform user it was already executed |
| Small prompt ("see Q(N).txt") + already addressed | **Abort** — likely human error, suggest checking file name |
| Large prompt with new context + already addressed | Check if user wants extension; if yes, new Q file; if no, abort |
| User asks to build/make/create a guide (any action word + "guide") with existing A files | **DO NOT abort** — this is legitimate guide compilation |
| User creates new Q file saying "follow up on Q5" | **DO NOT abort** — new Q file gets new A file |

### Human Error Protection

**When a user accidentally references the wrong file, the agent must detect this
and inform the user rather than blindly proceeding.**

#### Detection Criteria

Human error is likely when ALL of these are true:
1. The referenced file EXISTS
2. The file was ALREADY addressed (A file or E file exists in INDEX.md)
3. The user prompt is SMALL or SIMPLE (e.g., just "see Q2.txt" or "address Q2.txt")

#### Response to Likely Human Error

- Abort the task (do not create duplicate work)
- Reply informatively:
  "[File] was already addressed in [output file] on [date]. 
   If you meant a different file, please check the file name. 
   I have not created anything new."
- Create NOTHING in the context engine

#### Why This Matters

Without this protection, a user who accidentally re-references an old file would
get duplicate work created, wasting tokens and creating confusion in INDEX.md.

### Other Error Handling Rules

- If the user references a media file by name but the file is not in the correct
  media directory for that instance: stop. Reply with the error. Do not look in
  the other media directory. Do not invent a path.
- If the user appears to want a 4th interaction mode that is not defined here:
  do not invent it. Say so, and ask what they want within the three instances.
- If you are unsure which instance applies: ask. Do not guess and do not proceed
  on a guessed instance.
- If `INDEX.md` is missing: cannot proceed — ask the user to verify the module exists.
- If the instruction file for the chosen instance is missing: ask — rules cannot be guessed.

## Cross-repo protection

- **READ-ONLY access to sibling repos:** When reading another FYP-Desk repo (fyp-ideas, contribution-tracker-valueation, specilized-agents-skills-from-agency-agents), read files only. NEVER write inside another repo.
- **Write only to your own engine:** All writes (Q/A/T/E/tracker/log files, indexes)
  happen in THIS engine's directories only.
