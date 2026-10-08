# Dev Skills

Anthony Kirksey's reusable coding-agent skills for focused, repository-aware development. Follow existing conventions, keep changes small, and verify the result.

Compatible with the [Agent Skills format](https://agentskills.io/specification) and [Vercel's skills CLI](https://github.com/vercel-labs/skills).

## Skills

| Skill | Purpose |
| --- | --- |
| [coding-guidelines](skills/coding-guidelines/SKILL.md) | Go and Python standards for implementation, refactoring, debugging, and review. Includes preferred Go packages for new projects without overriding existing conventions. |

## Install

With Node.js and npm installed:

```bash
# List available skills
npx skills add tomato3017/dev-skills --list

# Install into your project
npx skills add tomato3017/dev-skills --skill coding-guidelines
```

Add `--global` to install across projects. Select your agent when prompted.

## Create a skill

- `skills/` — finished skills
- `inprogress/` — drafts
- `templates/` — authoring template

```bash
mkdir -p inprogress/my-skill
cp templates/SKILL.md.template inprogress/my-skill/SKILL.md
```

Fill in the template, then install the draft in a scratch project to test it:

```bash
npx skills add ./inprogress/my-skill
```

When ready, move it into `skills/`, update the table above, and check discovery:

```bash
mv inprogress/my-skill skills/my-skill
npx skills add . --list
```

Drafts can still be discovered with `--full-depth` or recursive fallback; `.template` files are not installable skills.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the authoring checklist and [AGENTS.md](AGENTS.md) for repository instructions.

## License

[MIT](LICENSE) — Copyright (c) 2026 Anthony Kirksey.
