# Working in Dev Skills

This is Anthony Kirksey's personal collection of reusable coding-agent skills, hosted at `tomato3017/dev-skills`. It is a private, Markdown-first repository compatible with Vercel's skills CLI—not an application or a Vercel deployment.

## Repository conventions

- Put finished, installable skills in `skills/<skill-name>/SKILL.md`.
- Put drafts in `inprogress/<skill-name>/`; promote them only when the user asks or explicitly approves.
- Keep the reusable skeleton in `templates/SKILL.md.template`, not a discoverable `SKILL.md`.
- Keep supporting documents and helpers inside their skill directory. Preserve existing paths such as `coding-guidelines/reference/`; do not rename directories solely for consistency.
- Follow [CONTRIBUTING.md](CONTRIBUTING.md) for frontmatter, authoring, safety, testing, and licensing requirements.
- Keep instructions focused, repository-aware, and actionable. Do not force preferred dependencies or tooling onto projects with intentional alternatives.
- Do not add an app, dependencies, build system, or publishing automation unless the task needs it.

## README maintenance

Keep [README.md](README.md) personalized to this repository, not generic starter boilerplate:

- Use the actual repository name and `tomato3017/dev-skills` in install commands.
- Describe the skills that actually exist, with relative links and concise descriptions in the available-skills table.
- Update the table when a skill is added, renamed, removed, or promoted from `inprogress/`.
- Keep the layout, draft workflow, authentication guidance, and repository visibility accurate when they change.
- Do not claim skills have been behavior-tested, externally audited, or endorsed by Vercel without evidence.
- Do not describe `inprogress/` as hidden or protected: full-depth scanning can discover drafts, and recursive fallback can do so when standard locations contain no skills.

## Checks before finishing

- Run `git diff --check` for tracked changes and check new files for trailing whitespace.
- Verify relative Markdown links and referenced supporting files exist.
- For skill changes, run `npx skills add . --list` and confirm the intended finished skills appear; check drafts using their explicit local paths.
- When instructions change behavior, try the skill with the intended agent in a scratch project where practical. Report what was actually checked and what was not.
- Review new content for secrets, personal information, private infrastructure details, and redistribution rights. Preserve third-party license notices; the repository uses [MIT](LICENSE).
- Do not commit, push, publish, change repository visibility, or grant access without the user's request or approval.
