# Dev Skills

Anthony Kirksey's collection of reusable coding-agent skills: practical instructions for how I want development work approached across projects.

The goal is consistent, repository-aware work—not forcing every project into the same stack. Skills should respect existing conventions, keep changes focused, and make verification part of the job.

These are Markdown instruction sets, not an application or a Vercel deployment. They follow the [Agent Skills format](https://agentskills.io/specification) and can be installed with [Vercel's skills CLI](https://github.com/vercel-labs/skills). This repository is not affiliated with Vercel.

## Available skills

| Skill | What it covers |
| --- | --- |
| [coding-guidelines](skills/coding-guidelines/SKILL.md) | Implementation, refactoring, debugging, and review using Go and Python standards. Includes preferred Go packages for new work, without overriding an existing project's choices. |

## Install

This repository is currently **private**. You need access to `tomato3017/dev-skills` and working GitHub authentication. The CLI uses your configured Git credentials, GitHub CLI authentication, or SSH; installing a skill does not grant repository access.

With Node.js and npm installed, run from the project where you want to use the skill:

```bash
# See what's available
npx skills add tomato3017/dev-skills --list

# Install coding guidelines for this project
npx skills add tomato3017/dev-skills --skill coding-guidelines

# Or install globally for use across projects
npx skills add tomato3017/dev-skills --skill coding-guidelines --global
```

If you use SSH authentication:

```bash
npx skills add git@github.com:tomato3017/dev-skills.git --skill coding-guidelines
```

Select your intended agent when prompted. Installation makes the instructions available to that agent; it does not run a formatter, linter, or test suite itself.

## Working on skills

```text
skills/                    # Finished, installable skills
  coding-guidelines/
    SKILL.md
    reference/             # Go/Python standards and preferred Go stack
inprogress/                # Skills being drafted and tried out
templates/                 # Reusable authoring skeleton
  SKILL.md.template
```

Start a draft from the template:

```bash
mkdir -p inprogress/my-skill
cp templates/SKILL.md.template inprogress/my-skill/SKILL.md
```

Replace the placeholders with a specific purpose, clear activation conditions, actionable instructions, and a concrete example. Keep any supporting references or scripts inside the skill's directory and link them from `SKILL.md`.

Test a draft directly:

```bash
npx skills add ./inprogress/my-skill --list
```

Try installing it in a scratch project and exercising a normal request, an edge case, and a request that should not activate it. Once it's ready, move the whole directory into `skills/` and add it to the table above:

```bash
mv inprogress/my-skill skills/my-skill
npx skills add . --list
```

`inprogress/` is an organizational boundary, not access control: `--full-depth` scanning can discover drafts, and recursive fallback can discover them if there are no skills in standard locations. Anyone with repository access can read them. The template's `.template` extension keeps it out of discovery.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the authoring checklist and [AGENTS.md](AGENTS.md) for instructions to agents working in this repository.

## Sharing responsibly

Before adding or sharing a skill, check it for credentials, personal information, private infrastructure details, and content you don't have permission to redistribute. Preserve required third-party notices.

Private access limits who can fetch the repository; it does not prevent recipients from copying installed skill files.

## License

[MIT](LICENSE) — Copyright (c) 2026 Anthony Kirksey.
