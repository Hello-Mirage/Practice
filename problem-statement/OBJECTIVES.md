# Objectives and Success Metrics

## Primary objective

Deliver a working desktop AI chatbot that can invoke useful modular skills while confining their file operations to a dedicated, security-enforced Damage Radius workspace.

## Supporting objectives

1. Make it easy to add and invoke skills through a documented interface.
2. Prevent skills from accessing or modifying files outside their granted workspace.
3. Make consequential actions visible, permissioned, and auditable.
4. Add voice input/output and optional wake-word activation if the secure MVP is stable.

## Success metrics

| Metric | Target | How it will be measured |
|---|---:|---|
| Core chat flow | Works end to end | Send prompt and display model response |
| Skill invocation | At least 2 working skills | Demonstrate e.g. list/search and create-note |
| Workspace boundary | 100% of escape tests blocked | Automated path traversal, symlink, and unauthorized-path tests |
| Destructive action control | Confirmation required | Test delete/overwrite flows |
| Auditability | Every skill invocation logged | Inspect logs for actor, skill, action, result, timestamp |
| Voice bonus | Speech-to-text and text-to-speech demo | Complete a spoken request and hear a response |
| Wake-word bonus | Local activation demo | Confirm activation without continuously sending audio to a remote service |

## Definition of done

- [ ] Desktop app starts and supports a basic chat conversation.
- [ ] Model tool calls are treated as untrusted requests, not executable authority.
- [ ] Skills are registered through a controlled interface.
- [ ] At least two useful skills work within the Damage Radius.
- [ ] The runtime enforces filesystem isolation independently of model instructions.
- [ ] Path traversal, symlink, and unauthorized access tests pass.
- [ ] Destructive operations require clear user approval.
- [ ] Skill invocations and outcomes are logged without logging secrets.
- [ ] Voice input and output work if time permits.
- [ ] Wake-word detection is optional, clearly indicated, and privacy-conscious.
