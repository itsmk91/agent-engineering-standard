# An engineering standard for AI agents

**AI agents can build software that works today and is painful to change next month. A better prompt doesn't fix that. A standard the agent follows from the first file does.**

This is that standard, packaged as two Agent Skills. The first makes structure, clear ownership, tests, documentation and Git hygiene part of the build instead of a clean-up job for later — and it scales down, so a small project stays small. The second decides how finished work is handed over: with proof another agent can check, not a bare "done".

The build standard is tool-agnostic. Nothing in it depends on a particular model, CLI, or framework.

Both skills are in this repo — [`skills/`](skills).

---

## In practice

![How a builder agent works under the standard: the board around it, its eight steps, the folder it leaves behind, and the rules it keeps](media/how-i-build-a-new-project.png)

*The standard at work on the author's own board. One agent plans, one writes the exam and reviews the code, one builds — and a person holds every gate.*

![How a builder agent hands over finished work: where the handoff sits, one small fix handed over line by line, the words it writes instead of "done", and the rules it keeps](media/how-i-hand-over-finished-work.png)

*The handoff at the end of every build. Each line says what kind of line it is — a fact, evidence, a claim, or not checked yet — and then the builder stops.*

---

## What it asks of an agent

- **Size the work first.** A typo gets the smallest safe change. A new project gets its requirements, a map of its parts, a data model and a test plan before the first feature.
- **Give every job one home.** No god files. Product rules stay out of the screens and the storage.
- **Build the tests and the docs with the code**, not after it.
- **Don't over-engineer.** No layer, interface, framework or folder without a real problem it solves.
- **Leave the big calls to the owner.** Replacing the architecture, changing how data is stored, or anything that could lose data waits for a person.
- **Hand it over with proof, not "done".** Say what was checked and what wasn't, and where the evidence is — then stop, and let another agent check it.

---

## Use it

One line, for every project on your machine:

```bash
npx skills add itsmk91/agent-engineering-standard -g
```

Or copy them by hand:

```bash
git clone https://github.com/itsmk91/agent-engineering-standard.git
mkdir -p ~/.claude/skills
cp -R agent-engineering-standard/skills/software-engineering-build-standard ~/.claude/skills/
cp -R agent-engineering-standard/skills/engineering-builder-handoff ~/.claude/skills/
```

Codex reads `~/.codex/skills/` instead; the build standard ships the `agents/openai.yaml` it expects. The handoff is written for Claude as the builder, with Codex as the reviewer unless a project names another.

Agents load them on their own: the build standard when they build, extend, restructure or review software, the handoff when they finish. Each core is one page — the [build standard](skills/software-engineering-build-standard/SKILL.md) and the [handoff](skills/engineering-builder-handoff/SKILL.md) — and each has four reference files that load only when the work needs them. A project's own rules, its recorded decisions and the owner's instructions always come first.

---

## What it costs

- **Slower first steps.** A new project spends its opening on a map, a data model and a test plan instead of features.
- **More questions for the owner.** Big decisions wait for a person, and some of those waits will feel slow.
- **Longer endings.** A finished job ends with a handoff instead of one word — a few lines for a small fix, a full report for risky work.
- **It can't rescue a bad idea.** A standard shapes how something is built, not whether it was worth building.

**Worth it when:** the software has to outlive the week, or more than one agent or person will change it.

**Not worth it when:** it's a throwaway prototype. The standard itself says to keep those minimal.

---

## The shortest version

> Never build first and organize later. Give every piece a home, a check and a note from the first file — and keep it no more complicated than the product needs. Then hand it over with the proof, not the word "done".

---

<sub>by Workspace Labs · Drawn from a working system, not a thought experiment — see <a href="https://github.com/itsmk91/workspace">a showcase of it running</a>, and the patterns beside it: <a href="https://github.com/itsmk91/agent-health-checks">health checks for AI agents</a> and <a href="https://github.com/itsmk91/agent-separation-of-duties">separation of duties for AI agents</a>.</sub>
