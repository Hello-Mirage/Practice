# Skill Interface Specification

## Purpose

A skill is a small, named capability the assistant can request through the skill gateway. Skills must not call the model directly, decide their own permissions, or access the host machine outside the restricted runner.

## Skill metadata

Each skill should declare:
- `id`: stable identifier, e.g. `note_create`.
- `name`: human-readable name.
- `description`: what it does and does not do.
- `version`: semantic version.
- `input_schema`: strict JSON schema.
- `capabilities`: requested workspace operations.
- `network`: false by default.
- `requires_confirmation`: true for destructive or externally visible actions.
- `timeout_seconds`: bounded execution time.

## Invocation contract

The model may request a skill ID and arguments. The gateway must:
1. Confirm the skill is registered and enabled.
2. Validate arguments against the skill's schema.
3. Check user-granted permissions.
4. Decide whether confirmation is required.
5. Invoke the skill in the restricted runner.
6. Validate and size-limit the result.
7. Record the action in the audit log.

Never execute arbitrary code, shell strings, or model-generated file paths directly.

## Result format

Return structured results such as:

```json
{
  "ok": true,
  "summary": "Created notes/meeting.md",
  "data": {
    "relative_path": "notes/meeting.md"
  },
  "error": null
}
```

For failures, return a safe error code and a concise explanation. Do not include secrets or full sensitive file contents in logs.

## Initial skill set

| Skill ID | Purpose | Permissions |
|---|---|---|
| `workspace_list` | List files under an allowed workspace folder | Read/list |
| `workspace_search` | Search text in workspace files | Read |
| `note_create` | Create a new note | Create |
| `file_summarize` | Summarize a user-selected workspace file | Read |

## Developer rules

- Skills receive a workspace handle, not unrestricted host paths.
- Skills must not spawn shells or child processes in the MVP.
- Skills must not install packages or load arbitrary plugins at runtime.
- Keep each skill focused and independently testable.
- Treat file contents as untrusted data.
- Include unit tests and boundary tests for every filesystem skill.
