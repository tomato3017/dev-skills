# Contributing skills

## Structure

Put each finished skill in `skills/<skill-name>/SKILL.md`. Start with [the template](templates/SKILL.md.template), and use the same name for the directory and YAML `name` field.

Only add supporting files when the skill needs them:

- `references/` for detailed documentation loaded on demand.
- `scripts/` for executable helpers.
- `assets/` for files used in the output, such as templates.

Link supporting files from `SKILL.md` using paths relative to the skill directory. Do not put a `SKILL.md` in the repository root; it would describe the whole repository as one skill.

## Authoring checklist

- [ ] Frontmatter contains a unique `name` and a clear `description` stating both purpose and activation conditions.
- [ ] Name uses lowercase letters, numbers, and single hyphens, with no leading or trailing hyphens, and is at most 64 characters.
- [ ] Description is at most 1024 characters and is valid YAML; quote values containing YAML-significant punctuation.
- [ ] Instructions cover one focused task and have no template placeholders left.
- [ ] Prerequisites and any required tools are explicit; do not assume every agent has the same tools.
- [ ] Destructive actions, publishing, and changes to credentials require explicit user approval.
- [ ] There are no credentials, private information, or instructions to bypass security checks.
- [ ] Supporting files exist and are linked; detailed references stay outside the main instructions where practical.
- [ ] `SKILL.md` stays below 500 lines where practical.
- [ ] `npx skills add . --list` discovers the intended skill and does not list the template.
- [ ] The installed skill has been tried in a scratch project with a normal request, an edge case, and a request that should not activate it.
- [ ] Any executable helper has a small automated test or assert-based self-check, with the test command documented alongside it.

Keep contributions focused. Include the purpose of the skill and what you tested in the pull request.

## License

Contributions must be content you have permission to submit under this repository's [MIT License](LICENSE). Preserve any required third-party copyright and license notices.
