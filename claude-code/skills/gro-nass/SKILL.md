---
name: gro-nass
description: Write the claim before you land it, run the refuse-check before you say it is done, and read your own refusals. For repositories gated by gro-nass.
---

# Committing through the gate

<!-- gro-nass:skill:start v1 — generated from listings/SKILL.source.md, do not edit between the markers -->

Three habits, in order. They are cheap, they are mechanical, and each one exists because skipping it
has cost somebody a day.

## 1. Write the claim before you land it

Every commit carries a `## PRE-REGISTRATION` block: what you predict the checks will say, written
*before* they say it.

```
## PRE-REGISTRATION
by: <your name> (<your model>) · tree: <the sha `git write-tree` prints right now>

1. the whole suite is green on the staged tree [verify.exit == 0]
2. no rule fires on a changed line [scanner.hits == 0]
3. this block is machine-readable [prereg.ok == 1]
```

- **Stage first, then bind.** The block is about the STAGED tree. Write it before staging and the
  fix re-stages, the tree moves, and your numbers describe a tree that no longer exists.
- **Every item carries a probe** — `[name cmp value]`. A prediction a machine cannot settle is not a
  prediction, it is a sentence.
- **Derive each number from the tree in front of you.** Never carry one forward from a previous
  attempt. The tree moved; the number did not.
- **A miss refuses nothing.** Predict what you actually expect, not what you hope. A wrong
  prediction that is recorded is worth more than a safe one that is meaningless.

## 2. Run the refuse-check before you say it is done

"Done" is a claim, and it is the one most often wrong. Before you write it:

```
gro-nass check-staged                 # the code checks, seconds, no worktree
gro-nass wrap --check-message <file>  # the message checks, milliseconds
gro-nass wrap -F <file>               # land it
```

- The cheap checks refuse most often. Run them first and you spend seconds finding what a full
  verification would take minutes to tell you.
- **A green run you did not look at is not evidence.** Read the verdict line. If you did not read
  it, say so instead of saying done.
- If you cannot run the check, say that. "I could not verify this" is a useful sentence; "done" in
  its place is not.

## 3. Read your own refusals

A refusal is the most specific feedback you will get, and it is addressed to you.

- **Read the whole line.** It names the check, the file, the line and the rule. Every one of those
  is a fact you would otherwise have to go and find.
- **Fix the cause, not the symptom.** Suppressing a rule you have not understood converts a finding
  into a silence, and silence is what this whole apparatus exists to remove.
- **When you have been refused twice for the same reason, stop and re-read.** The second refusal is
  the tool telling you your model of the problem is wrong, not that your typing was.
- **Do not re-run hoping for a different answer.** These checks are deterministic. A run that goes
  green after a run that went red means something changed — know what.

**When you admit a defect, its class comes from a declared list.** A `## DEFECT` block naming a class
the tool does not declare is refused, and the refusal names it. The list and its definitions are in
`RULES.md`; `class` used to be free text, and a typo was a new class — one that groups with nothing
and reads to every later reader as a real finding.

- `scanner-finding` is for automated per-commit rows from a scanner, so bulk machine findings are not
  filed under a judgement class.
- `scope-excluded-silently` — a checker ran, reported clean, and its configured scope excluded the
  subject without saying so.
- `unbuilt-commitment` — something the plan or the record names as existing does not exist.
- `contention` — two things ran that could not both run.
- `teardown-not-guaranteed` — cleanup written where it does not run.
- `cross-clock` is merged into `wrong-clock`: accepted on read, refused on write.

## What never goes in a commit message

No attribution trailers. No `Co-Authored-By` naming a tool, no "generated with" line. The record of
who wrote what lives on the row, where it can be counted; a trailer is an unverifiable sentence in
prose and the gate refuses it.

<!-- gro-nass:skill:end -->
