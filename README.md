# Dev Skills

A starter repository for reusable AI agent skills, compatible with [Vercel's skills CLI](https://github.com/vercel-labs/skills) and the [Agent Skills specification](https://agentskills.io/specification).

This repository includes the **coding-guidelines** skill for repository-aware Go and Python coding standards, plus a template for authoring more skills. It is not affiliated with Vercel and does not require a Vercel deployment.

## Layout

```text
skills/                    # Published skills: one directory per skill
  coding-guidelines/       # Go and Python coding standards
templates/                 # Authoring template, not an installable skill
  SKILL.md.template
```

## Create a skill

From the repository root:

```bash
mkdir -p skills/my-skill
cp templates/SKILL.md.template skills/my-skill/SKILL.md
```

Edit the copied file:

- Set `name` to the directory name, using lowercase letters, numbers, and single hyphens (no leading or trailing hyphens; maximum 64 characters).
- Replace `description` with what the skill does and when an agent should use it (maximum 1024 characters).
- Replace the instructional placeholders with a focused workflow, safety checks, and a concrete example.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the authoring checklist.

## Check discovery locally

With Node.js and npm installed, run:

```bash
npx skills add . --list
```

This should list `coding-guidelines`. The template is deliberately named `SKILL.md.template` so the CLI does not discover or install it.

To test a completed skill in a separate scratch project:

```bash
npx skills add /absolute/path/to/dev_skills --skill my-skill
```

Select the agent you want to test with, then try a representative request and a request that should not activate the skill. Read any bundled scripts before executing them.

## Install from GitHub

After publishing and adding at least one completed skill, replace `OWNER/REPO` and `my-skill` below with your actual GitHub repository and skill name. Run these commands from the project where you want the skills installed:

```bash
# List available skills without installing
npx skills add OWNER/REPO --list

# Install one skill into the current project
npx skills add OWNER/REPO --skill my-skill

# Install one skill globally instead
npx skills add OWNER/REPO --skill my-skill --global
```

## Before publishing

- Test the skills with your intended agents.
- Confirm you have permission to redistribute imported content and preserve any required third-party notices.
- Review tracked files for credentials, private paths, or proprietary instructions.
- Create the GitHub repository, add its remote, and push when ready. No remote or publishing automation is configured here.

## License

[MIT](LICENSE) — Copyright (c) 2026 Anthony Kirksey.

## References

- [Skills CLI documentation](https://github.com/vercel-labs/skills#readme)
- [Agent Skills specification](https://agentskills.io/specification)
- [Skills directory](https://skills.sh)
