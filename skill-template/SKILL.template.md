---
# REQUIRED. 1-64 chars. Only a-z, 0-9, hyphen. No leading/trailing hyphen, no "--".
# Must match parent directory name exactly. Example: folder `ship/` -> `name: ship`.
# TODO: replace `my-skill-name` and rename the folder to the same value.
name: my-skill-name

# REQUIRED. 1-1024 chars. Third person. Must say WHAT it does AND WHEN to use it.
# This is the only thing the agent reads at startup (~100 tokens) to decide activation.
# Be specific + pushy. Bad: "Helps with PDFs." Good: see below.
# TODO: rewrite with your capability + 3-5 trigger phrases the user would really type.
description: Does X task with Y output. Use when user says "trigger phrase 1", "trigger phrase 2", or needs Z outcome.

# OPTIONAL but recommended for public skills. Short name or bundled file.
# Common: MIT, Apache-2.0. Must match root LICENSE file.
license: MIT

# OPTIONAL. Max 500 chars. Only if you have real requirements.
# Examples: "Requires git, docker, jq and internet access." / "Requires Python 3.11+."
# Omit if no special requirements (most skills omit it).
# compatibility: Requires git and internet access.

# OPTIONAL. Free-form string->string map. Use for versioning and authorship.
metadata:
  author: TODO-YOUR-USERNAME
  repository: https://github.com/TODO-YOUR-USERNAME/skills
  version: "0.1.0"
  category: TODO # e.g. development, workflow, documentation
  last-updated: TODO-YYYY-MM-DD

# OPTIONAL, EXPERIMENTAL. Space-separated pre-approved tools. Support varies per agent.
# Example: allowed-tools: Bash(git:*) Read
# Omit unless you know the target agent supports it.
---

# TODO: Human-readable Title

<!-- HOW TO USE THIS TEMPLATE
1. Copy this whole folder: `skill-template/` -> `skills/<tu-nombre>/`
2. Rename folder == `name:` above (kebab-case).
3. Replace every TODO in this file.
4. Keep SKILL.md under 500 lines / <5000 tokens. Move detail to references/.
5. Validate: `npx skills add TU-USUARIO/skills --list` + `skills-ref validate ./skills/<tu-nombre>`
6. Delete sections you don't need. A minimal skill only needs: frontmatter + Instructions + 1 example.
Spec: https://agentskills.io/specification
-->

TODO: 1-3 sentences. What does this skill do, why is it useful, who should use it.
Write in imperative voice for the agent, explain the WHY, not just MUSTs.

## When to Use

Use this skill when:

- TODO: trigger condition 1 (user phrase or context)
- TODO: trigger condition 2
- TODO: trigger condition 3

Do NOT use when:

- TODO: adjacent case where another skill/tool is better
- TODO: simple one-step task the agent can do without this skill

## Prerequisites

TODO: delete this section if none. Otherwise list what must exist before running.

- TODO: required tool, env var, file, or access (e.g. `git`, `GH_TOKEN`)
- Check with: `TODO: command to verify, e.g. git --version`

## Instructions

Follow these steps in order. Prefer the imperative form.

1. **TODO Step 1 (Inspect):** TODO: what to read/check first. Stop if TODO: condition.
2. **TODO Step 2 (Act):** TODO: exact command or action. Example: `TODO: command --flag`.
3. **TODO Step 3 (Verify):** TODO: how to confirm success before finishing.
4. **TODO Step 4 (Report):** TODO: what to tell the user at the end (2-3 lines max).

```text
TODO: optional copyable progress checklist for multi-step workflows.
Progress:
- [ ] Step 1 done
- [ ] Step 2 done
- [ ] Step 3 done
```

## Input / Output Examples

**Example 1:**

Input: TODO: realistic user request, e.g. "Sube mis cambios al repo"

Output: TODO: exact expected result

```text
TODO: paste a real before/after or command transcript
```

**Example 2 (edge case):**

Input: TODO: tricky variant

Output: TODO: how the skill should handle it

## Output Format

TODO: delete if free-form. If the agent must produce a fixed shape, give a copyable template
(agents pattern-match better against concrete templates than prose).

```markdown
# [TODO Title]

## Summary
TODO: one line

## Details
TODO: bullets

## Next steps
TODO: list
```

## Edge Cases

- TODO: edge case 1 -> TODO: what to do (e.g. "two unrelated topics in one diff -> ask before splitting commits")
- TODO: edge case 2 -> TODO: what to do

## Error Handling

| Error | Cause | Solution |
| --- | --- | --- |
| TODO: e.g. hook rejects commit | TODO | TODO: show error, stop, never use `--no-verify` without asking |
| TODO | TODO | TODO |

## References

TODO: progressive disclosure. Keep SKILL.md lean; move detail here.
Agents load these files ONLY when needed, so smaller files = less context used.
Link each file with WHEN to read it. Keep refs one level deep, no nested chains.

- Read [REFERENCE.md](references/REFERENCE.md) when TODO: condition (detailed spec, schemas, API docs).
- Read [EXAMPLES.md](references/EXAMPLES.md) when TODO: need more than the 2 inline examples above.

Related scripts (run, don't paste into context):

- `scripts/TODO-example.sh` — TODO: what it does. Run with `bash scripts/TODO-example.sh --help`.

Related assets (used in output, not loaded into context):

- `assets/TODO-template.md` — TODO: template the agent copies/modifies.

## Safety and Limitations

- ALWAYS TODO: what must always happen (e.g. check for secrets before commit).
- NEVER TODO: what must never happen (e.g. force-push, amend, exfiltrate tokens).
- This skill cannot: TODO: honest limitation 1, limitation 2.

---

© TODO-YEAR TODO-YOUR-USERNAME. License MIT. Canonical repo: https://github.com/TODO-YOUR-USERNAME/skills.
