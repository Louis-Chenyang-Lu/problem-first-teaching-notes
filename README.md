# Problem-First Teaching Notes

**Turn any concept into a student-ready note that starts from the problem it solves, not from its definition.**
An agent skill compatible with **Claude** (Claude.ai, desktop, Claude Code) and **OpenAI Codex** (CLI, IDE extension, ChatGPT desktop app).

> **中文简介：** 一个写讲义的 Skill：从动机和要解决的问题出发，先看手上有什么、先试最直接的想法，再让定义和方法自然出现，而不是先给定义再证明。详见下方 [中文摘要](#中文摘要)。

---

## Why this skill

Most teaching material is written in the order a textbook stores knowledge: definition, assumptions, theorem, proof. Students can follow each line and still have no idea why anyone would define the object that way. This skill writes notes in the order a researcher would *discover* the idea.

| Definition-first (typical) | Problem-first (this skill) |
|---|---|
| "The graph Laplacian is defined as L = D − W." | "We have a network and a value on every node. How do we score whether the values respect the network?" |
| Assumptions appear as decrees | Each assumption is a choice, with its reason |
| The naive alternative is never mentioned | The obvious first idea is tried and shown to fail, with numbers |
| The name comes first | The name comes last, as a label for something already understood |
| Examples come after the theory | One running example carries the whole note |

## How it works

```
Real-world setting ──► What we want to know ──► What we actually have
        │
        ▼
Naive first idea ──► why it fails (concretely) ──► better model, each choice justified
        │
        ▼
Formal object appears and gets its name ──► key result, simplest case first
        │
        ▼
Worked example with real numbers ──► limits ──► recap of the chain ──► check questions
```

## Key features

- **Motivation before definition.** A note never opens with "X is defined as".
- **One running example** introduced early and used throughout, with numbers small enough to check by hand.
- **Productive failure.** The naive approach is shown to break, so the real definition is the fix the reader was waiting for.
- **Explicit modelling choices.** Every assumption comes with the reason for making it.
- **Verified mathematics.** Derivations are re-checked, and worked-example numbers are computed with code when the agent can run it.
- **Presentation-ready.** Every section opens with a bold one-sentence takeaway, ready to become a slide title.
- **Honest limits.** Each note ends with what the concept does not tell you and common misreadings.

## Installation

The repository contains two editions of the same skill. They differ only in how the note is delivered.

| Edition | Folder | Delivers the note as |
|---|---|---|
| Claude | `skills/claude/problem-first-teaching-notes` | An editable document when available, otherwise Markdown with LaTeX |
| Codex | `skills/codex/problem-first-teaching-notes` | A Markdown file with LaTeX in your workspace; includes `agents/openai.yaml` |

### Claude.ai / Claude desktop app
1. Download `problem-first-teaching-notes-claude.zip` from the [Releases page](https://github.com/Louis-Chenyang-Lu/problem-first-teaching-notes/releases), or build it yourself:
   ```bash
   git clone https://github.com/Louis-Chenyang-Lu/problem-first-teaching-notes.git
   cd problem-first-teaching-notes/skills/claude
   zip -r problem-first-teaching-notes-claude.zip problem-first-teaching-notes
   ```
2. Upload the zip as a skill in Claude. See the official guide: [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude).

### Claude Code
```bash
git clone https://github.com/Louis-Chenyang-Lu/problem-first-teaching-notes.git
mkdir -p ~/.claude/skills
cp -r problem-first-teaching-notes/skills/claude/problem-first-teaching-notes ~/.claude/skills/
```

### OpenAI Codex (CLI / IDE extension / ChatGPT desktop app)
```bash
git clone https://github.com/Louis-Chenyang-Lu/problem-first-teaching-notes.git
mkdir -p ~/.agents/skills
cp -r problem-first-teaching-notes/skills/codex/problem-first-teaching-notes ~/.agents/skills/
```
For a single project, copy it to that repository's `.agents/skills/` instead. Restart Codex if the skill does not appear. Trigger it in natural language or with `$problem-first-teaching-notes`. See [Build skills](https://learn.chatgpt.com/docs/build-skills) in the Codex documentation.

## Quick start

```
Use problem-first-teaching-notes to write a teaching note on <concept>.
My students are <level> and already know <prerequisites>.
```
More: [Usage guide](docs/usage-guide.md) · [Prompt templates](examples/prompts.md) · [Full example note: the graph Laplacian](skills/claude/problem-first-teaching-notes/references/example-graph-laplacian.md)

## Repository structure

```
problem-first-teaching-notes/
├── README.md
├── LICENSE
├── docs/
│   └── usage-guide.md                     # when to use, prompting, note structure, FAQ
├── examples/
│   └── prompts.md                         # copy-paste prompt templates
└── skills/
    ├── claude/problem-first-teaching-notes/   # Claude edition
    │   ├── SKILL.md
    │   └── references/example-graph-laplacian.md   # a complete example note
    └── codex/problem-first-teaching-notes/    # Codex edition
        ├── SKILL.md
        ├── agents/openai.yaml             # Codex UI metadata
        └── references/example-graph-laplacian.md
```

## Limitations

- **Best for concepts with a motivating problem.** Definitions, methods, estimators, transforms and theorems fit well. Pure reference material (formula sheets, API docs) does not benefit much.
- **The story is a reconstruction, not history.** Notes present a route by which one *could* arrive at the concept; ask explicitly if you want the historical development.
- **Review before teaching.** The skill re-derives steps and checks numbers when it can run code, but you should still read the note as you would any teaching material.
- **Notes only, for now.** Each section is slide-ready, but generating a slide deck is not part of this release.

## 中文摘要

**它做什么。** 给定一个概念、方法或定理，生成一份学生可以直接阅读的讲义。讲义从现实问题和动机出发：问题出现在哪里、我们想知道什么、手上有什么数据和工具、最直接的想法为什么不够、怎样一步步改进出合理的模型，最后才给出正式定义和名称，再配上可以手算的例子、局限性和小结。

**安装。** Claude：在 Releases 下载 `problem-first-teaching-notes-claude.zip` 并上传为 Skill。Claude Code：把 `skills/claude/problem-first-teaching-notes` 复制到 `~/.claude/skills/`。Codex：把 `skills/codex/problem-first-teaching-notes` 复制到 `~/.agents/skills/`。

**语言。** 讲义默认使用提问时的语言，也可以指定中文或英文。

## License

MIT. See [LICENSE](LICENSE).
