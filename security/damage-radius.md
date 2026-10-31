# Damage Radius: Security Boundary

## What it means

The **Damage Radius** is a dedicated workspace where skills are allowed to perform file operations. It is the maximum area of user data that a skill can affect. It is not a magic folder name and must not rely on the AI model voluntarily staying inside it.

Example layout:

```text
app-data/
├── config/                 # App configuration; not mounted into skills
├── logs/                   # Protected audit logs; not writable by skills
└── damage-radius/           # Skill workspace
    ├── inbox/
    ├── notes/
    └── output/
```

## Required enforcement

- Run skills as a separate low-privilege process or container.
- Expose only the Damage Radius path to that process.
- Do not mount the user's home directory, app configuration, credential stores, SSH keys, browser profiles, or unrelated paths.
- Deny network access by default. If a skill genuinely needs network access, use a narrowly scoped broker/allowlist and disclose it to the user.
- Apply execution timeouts and resource limits.
- Keep secrets out of skill environment variables and tool results.
- If the platform sandbox is unavailable, fail closed and do not run the skill.

## File operation rules

1. Accept relative paths only.
2. Resolve and validate paths against the workspace root.
3. Reject traversal sequences and absolute paths.
4. Prevent symlink escapes; do not assume string-prefix checks are sufficient.
5. Use safe filesystem APIs and avoid check-then-use races.
6. Limit file sizes and supported file types.
7. Require explicit approval before delete, overwrite, bulk modification, or external transfer.
8. Prefer preview/dry-run for multi-file changes.
9. Keep protected audit records of requested and completed actions.

Path validation in application code is useful defense in depth, but the OS sandbox is the actual boundary.

## Permission model

Every skill declares:
- ID, name, version, and description.
- Input schema.
- Filesystem capabilities: read, create, update, delete.
- Allowed subdirectories, if narrower than the workspace.
- Network requirement (default: none).
- Whether user approval is required.
- Maximum runtime and resource budget.

The registry declaration does not itself grant permission. The user/policy engine grants permissions, and the runner enforces them.

## Threats and mitigations

| Threat | Mitigation |
|---|---|
| Prompt injection in a document | Treat document text as data; never as policy or permission |
| Malicious or buggy skill | Isolated runner, least privilege, review, resource limits |
| Path traversal | Canonical path checks plus OS-enforced mount boundary |
| Symlink escape | No-follow/descriptor-based safe operations and sandbox |
| Destructive model mistake | Preview and user confirmation |
| Secret leakage | No secret mounts, redaction, no secrets in prompts/logs |
| Network exfiltration | Network denied by default |
| Infinite loop/resource exhaustion | Timeout and CPU/memory limits |

## Security acceptance tests

- Attempt `../../outside.txt` and confirm denial.
- Attempt an absolute host path and confirm denial.
- Create a symlink inside the workspace pointing outside and confirm it cannot be followed to read/write outside data.
- Attempt to read app configuration and user home files and confirm denial.
- Attempt network access from a skill and confirm denial by default.
- Force a skill crash or timeout and confirm the host app remains responsive.
- Confirm delete/overwrite actions cannot proceed without approval.

Document the OS, sandbox technology, and test evidence before claiming isolation is complete.
