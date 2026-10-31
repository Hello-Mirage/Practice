# Sentinel — AI Desktop Assistant

A security-first AI desktop chatbot with an extensible skills system. Users interact with an AI assistant through chat, and optionally voice. The assistant can invoke approved skills, but skill file operations are confined to a dedicated **Damage Radius** workspace rather than the user's whole computer.

## The problem

AI assistants become much more useful when they can take actions, use tools, and manipulate files. But granting an AI or its skills broad access to a desktop creates a serious security risk: an incorrect instruction, malicious prompt, compromised skill, or bug could damage or expose unrelated user data.

## The proposed solution

Build a desktop chatbot that:
- Provides a conversational AI interface.
- Loads discrete, documented skills through a controlled skill registry.
- Runs skill actions inside a restricted Damage Radius workspace.
- Uses explicit permissions, validation, confirmation for risky actions, and an audit log.
- Offers voice input/output and an optional wake-word activation path as bonus features.

**Security principle:** the Damage Radius is a boundary enforced by the operating system or sandbox—not merely a folder convention. Skills must not be able to escape it.

## Quick start for the project

1. Read [the problem statement](problem-statement/PROBLEM.md).
2. Review [MVP objectives](problem-statement/OBJECTIVES.md) and [requirements](problem-statement/REQUIREMENTS.md).
3. Read the [Damage Radius security design](security/damage-radius.md).
4. Review the [skill interface](skills/SKILL_SPEC.md).
5. Explore the [voice and wake-word plan](voice/VOICE.md).
6. Update the [architecture](design/architecture.md) as implementation decisions are made.

## Repository structure

```text
.
├── problem-statement/   # Problem, objectives, requirements, evaluation
├── security/            # Threat model and Damage Radius boundary
├── skills/              # Skill contract and examples/specification
├── voice/               # Voice API and wake-word bonus plan
├── research/            # Research notes and references
├── design/              # Architecture and diagrams
├── src/                 # Application source code
├── docs/                # Setup and progress documentation
├── presentation/        # Hackathon pitch material
└── .github/             # Issue and pull request templates
```

## Build order

**Secure execution boundary → Chat UI and model connection → Skill registry → First safe skill → Tests → Voice bonus**

Do not start by giving the model shell access or unrestricted filesystem tools. Prove the sandbox boundary first.

## Project details

- **Project name:** Sentinel (working title)
- **Type:** AI desktop assistant
- **Primary interface:** Desktop chat
- **Bonus:** Voice API, text-to-speech, and optional wake word
- **Team / hackathon:** TBD

## Submission checklist

- [ ] Chat interface can send and display responses.
- [ ] Skills are registered and invoked through a controlled interface.
- [ ] File operations are confined to the Damage Radius.
- [ ] Path traversal and symlink escape tests pass.
- [ ] Risky actions require user confirmation.
- [ ] Skill actions are logged.
- [ ] Voice input/output works (bonus).
- [ ] Wake-word activation works without always recording/transmitting speech (bonus).

See [CONTRIBUTING.md](CONTRIBUTING.md) for collaboration conventions.
