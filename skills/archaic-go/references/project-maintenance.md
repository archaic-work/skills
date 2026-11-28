# Project maintenance skill

Use a project-specific skill as the actionable maintenance playbook for both human maintainers and coding agents. Keep it versioned alongside the project and give it an unambiguous project-specific name.

- Create a dedicated skill for every project, teaching both human maintainers and coding agents how to maintain and extend it. Keep the skill versioned with the project and name it for that project.
- Use the project skill as the canonical maintainer playbook. Keep a concise README for human orientation, installation and usage; avoid duplicating detailed maintenance instructions in parallel documents.
- Cover purpose, architecture and invariants, code navigation, environment and configuration, build/test/run commands, common changes and extension points, debugging, and release or operational procedures as applicable.
- Keep `SKILL.md` concise with clear triggering metadata; put detailed procedures and reusable helpers in linked references or scripts. Make the entry point discoverable from the repository's existing agent instructions or a minimal link.
- Read the project's skill before changing the project and update it alongside changes to behavior, architecture, tooling, or maintenance procedures. For an existing project without one, create it from verified repository facts; do not invent commands or procedures.
- For projects with browser UI scenarios, record fixtures, browser test commands, screenshot capture and PR review in the project skill; follow the separate `archaic-go-visualtest` skill when available.

## Review checklist

- Do the documented paths and commands exist at the current revision?
- Can a new maintainer locate the relevant code, run tests, change a feature, debug a failure, and understand the release process?
- Are project-specific secrets, services and configuration described without exposing credentials?
- Are deployment and CI instructions grounded in the current owning repositories? Link to operational procedures rather than copying mutable infrastructure details.
- Does the repository README provide a human-friendly entry point without duplicating the full maintenance playbook?
- Were the skill's instructions updated when behavior or tooling changed?
