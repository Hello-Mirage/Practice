# Requirements and Scope

## MVP: must have

| ID | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| MVP-01 | Desktop chat interface | Must | User can send a message and see the assistant response |
| MVP-02 | AI provider adapter | Must | API key is stored outside source control; provider errors are handled |
| MVP-03 | Skill registry | Must | App lists approved skills and their declared capabilities |
| MVP-04 | Skill invocation gateway | Must | Model requests are schema-validated and permission-checked before execution |
| MVP-05 | Damage Radius workspace | Must | Skills can access only the configured workspace, enforced by OS sandbox/container or equivalent |
| MVP-06 | Safe file operations | Must | Read/list/create/update operations validate paths; delete/overwrite require confirmation |
| MVP-07 | Audit log | Must | Each invocation records skill, action, time, approval, and outcome |
| MVP-08 | Fail-closed behavior | Must | If sandboxing or permission checks fail, the skill does not execute |

## Should have

- Skill metadata: name, description, version, input schema, declared permissions.
- User-facing permission prompt showing the exact action and target.
- Cancellation and execution timeout.
- Tests for path traversal, symlink escape, oversized files, and invalid arguments.
- A simple settings screen for model configuration and workspace location.

## Bonus features

- Speech-to-text input through a voice API or local speech engine.
- Text-to-speech responses.
- Optional wake-word detection to activate listening.
- Visible microphone/listening state and an obvious mute/disable control.
- Push-to-talk fallback if wake-word detection is unavailable.

## Could have

- Skill import/export and developer documentation.
- Per-skill permission settings.
- Read-only skills and preview/dry-run mode.
- Local model support.

## Will not build in the MVP

- Arbitrary shell or PowerShell execution.
- Unrestricted access to the host filesystem.
- Automatic access to browser cookies, password stores, SSH keys, or other credentials.
- Silent deletion, overwriting, uploading, or external sharing.
- A claim that prompt instructions alone provide a security boundary.

## Functional requirements

1. The user can chat with the assistant from a desktop application.
2. The assistant can request a skill by a registered identifier and structured arguments.
3. The gateway validates the skill identifier, argument schema, user permissions, and target paths.
4. Skills execute in a restricted runtime with only the Damage Radius mounted or exposed.
5. Skills return structured results to the assistant; they do not receive direct access to model credentials.
6. The app asks the user before destructive or externally visible actions.
7. The user can inspect recent skill activity and cancel eligible running tasks.
8. If voice is enabled, the user can speak a request and hear the response.

## Security and non-functional requirements

- **Least privilege:** Each skill gets only the capabilities it needs.
- **Filesystem isolation:** The workspace boundary is enforced by OS-level sandboxing, containers, restricted process identity, or equivalent—not just string checks.
- **Path safety:** Canonicalize paths; reject traversal; prevent symlink escapes and time-of-check/time-of-use races.
- **Network:** Deny by default for skills; allow only explicitly approved destinations when needed.
- **Secrets:** Keep API keys in OS credential storage or a protected backend configuration; never pass secrets into skill context or logs.
- **Execution limits:** Apply timeouts, file-size limits, and CPU/memory limits where supported.
- **Audit:** Record skill, action, scope, approval, and result. Redact secrets and sensitive content.
- **Privacy:** Voice capture is opt-in; show listening state and provide a physical/UI mute or disable control.
- **Reliability:** Handle model/API failures, malformed tool calls, permission denial, and skill crashes safely.

## Constraints

- Hackathon deadline and team capacity.
- Desktop OS and packaging target are to be selected.
- AI, speech, and wake-word provider availability/cost.
- OS-level sandbox guarantees differ across platforms.
