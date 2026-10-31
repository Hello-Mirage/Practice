# Problem Statement: Secure AI Desktop Chatbot

## 1. Problem in one sentence

People want an AI desktop assistant that can do useful work through extensible skills, but giving an AI agent or its tools access to a user's computer creates unacceptable risks of accidental damage, unauthorized access, and data exposure.

## 2. Background and context

A chatbot that only generates text has limited ability to help with real desktop tasks. Skills—small capabilities such as organizing files, searching a workspace, summarizing documents, or creating notes—make an assistant more useful.

However, skills can have side effects. A poorly constrained skill, malicious input, prompt injection in a document, or model mistake could read, overwrite, delete, or transmit files outside the intended task.

The project explores a safer design: a desktop assistant whose skills operate inside a dedicated **Damage Radius** workspace with enforced boundaries and explicit permissions.

## 3. Who is affected?

- **Primary users:** People who want an AI assistant to perform practical desktop tasks.
- **Secondary stakeholders:** Developers who create or install skills; organizations deploying desktop AI tools.
- **Current alternatives:** General-purpose chatbots, desktop automation tools, and agent frameworks with varying permission models.
- **Out of scope:** A fully autonomous agent with unrestricted control over the host computer.

## 4. Why does it matter?

Useful automation should not require trusting every model output or third-party skill with the user's entire machine. A mistake should have a small, predictable blast radius. Users should know what a skill can access, approve consequential actions, and inspect what happened.

## 5. Proposed product

A desktop chatbot that connects to an AI model and exposes approved capabilities through a skill system. Skills run in a restricted environment and can access only the dedicated Damage Radius workspace and explicitly granted resources.

The assistant should:
- Explain and request permission before consequential actions.
- Validate every tool argument independently of the model.
- Keep a record of skill invocations and file changes.
- Fail closed when permissions or sandbox guarantees are unavailable.

## 6. Evidence and assumptions to validate

| Claim / assumption | How to validate |
|---|---|
| Users want assistants to perform bounded desktop tasks | Short user interviews and task tests |
| A dedicated workspace is understandable to users | Usability test the setup and approval flow |
| Skills can be useful without unrestricted host access | Demonstrate several workspace-only skills |
| Users understand permission prompts | Test whether users can predict each action's effect |

## 7. Problem boundaries

**In scope**
- Desktop chat interface.
- AI model integration through a backend/provider adapter.
- Skill discovery, registration, permission checks, and execution.
- Dedicated Damage Radius directory.
- Sandboxed file access and safe file operations.
- User confirmation for destructive or externally visible actions.
- Audit log and error handling.
- Bonus voice input/output and wake-word activation.

**Out of scope for the MVP**
- Unrestricted shell, terminal, or arbitrary host automation.
- Silent access to the user's home directory, browser profile, credentials, or personal files.
- Autonomous network access by every skill.
- Claims of perfect security or immunity to prompt injection.

## 8. Draft problem statement

We want to build a desktop AI chatbot that can perform useful tasks through modular skills. Because those skills may read or modify data, the assistant must not receive unrestricted access to the host machine. The system will confine skill operations to a dedicated Damage Radius workspace, enforce permissions outside the language model, require confirmation for high-impact actions, and provide visibility into what each skill does. Voice interaction and wake-word activation are optional bonus capabilities.
