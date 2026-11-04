# SwitchOps

**A multi-agent AI incident response workflow with coordinated investigation, independent review, human-in-the-loop approvals, and structured incident reporting.**

SwitchOps is a demonstration of how specialized AI agents can collaborate during an operational incident. It uses Switch to coordinate locally running Claude Code agents and a Telegram-connected command center. The workflow is designed to help an incident lead organize investigation, challenge assumptions, track approval boundaries, and produce a clear incident report.

> **Project status:** This repository currently documents the SwitchOps agent workflow and its demonstrated incident-response simulation. It does not yet contain an exported copy of the local Switch configuration or agent working directories. The INC-001 scenario described below was a simulation; no production systems were accessed or changed.

## 🎥 Project Demo

Watch the end-to-end SwitchOps walkthrough:

**[▶ Watch the SwitchOps demo recording (MP4)](https://github.com/vaishalisen2002/switchops/releases/download/v1.0-demo/SwitchOps.mp4)** · [View the GitHub release](https://github.com/vaishalisen2002/switchops/releases/tag/v1.0-demo)

The recording demonstrates:
- **Coordinator:** incident intake, triage, and task delegation.
- **Investigator:** technical investigation and ranked root-cause hypotheses.
- **Reviewer:** independent validation, challenges, and risk assessment.
- **Reporter:** consolidated incident reporting.
- **Human-in-the-loop:** approval boundaries and controlled decision-making.

*The video uses a simulated UPI payment gateway incident (INC-DEMO-001). No production systems were accessed or modified.*

## Why SwitchOps?

Incident response often requires several activities to happen in parallel:

- Coordinating the response and assigning investigation tasks.
- Investigating possible causes without treating assumptions as facts.
- Independently reviewing evidence, risks, and proposed mitigations.
- Communicating the situation and decisions clearly to stakeholders.

SwitchOps separates these responsibilities across focused agents while keeping consequential decisions under human control.

## 🏗️ System Architecture

SwitchOps follows an end-to-end, human-led multi-agent incident response workflow. The diagram below will illustrate the communication between the human incident commander, Telegram, Switch orchestration, the four Claude Code agents, and the human approval checkpoints across the incident lifecycle.

<!-- System architecture image will be added here once provided. -->

## Multi-agent architecture

The workflow uses four specialized agents in a shared **SwitchOps Command Center** room.

| Agent | Responsibility |
|---|---|
| `switchops-coordinator` | Leads triage, delegates work, consolidates findings, tracks decisions, and maintains incident status. |
| `switchops-investigator` | Develops and ranks technical hypotheses, identifies evidence needed, and proposes safe investigation or remediation plans. |
| `switchops-reviewer` | Independently challenges assumptions, checks risks, identifies missing evidence, and reviews proposed actions. |
| `switchops-reporter` | Produces an executive summary, timeline, findings, decisions, risks, and follow-up actions. |

### High-level workflow

1. The human incident lead reports an incident in the command center.
2. The Coordinator establishes the incident context and delegates investigation and review.
3. The Investigator develops hypotheses and identifies evidence required to confirm or rule them out.
4. The Reviewer independently evaluates the reasoning, proposed mitigations, and operational risks.
5. The Coordinator consolidates findings and records decisions and approval boundaries.
6. The Reporter prepares a draft incident report.
7. The Investigator and Reviewer check the report; the Reporter incorporates corrections.
8. The Coordinator reviews the final report. The human remains responsible for approving consequential actions.

## Technology and integrations

- **Switch** — agent orchestration, shared room, and local agent sessions.
- **Claude Code** — execution provider for the locally configured agents.
- **Telegram** — chat interface connected to the SwitchOps command center.

The agents were configured to run locally on the user's computer. This repository does not include credentials, Telegram bot tokens, private configuration, or local agent working directories.

## Human-in-the-loop safety

SwitchOps is designed to distinguish between analysis and execution.

- Separate confirmed facts from hypotheses and recommendations.
- Do not fabricate logs, metrics, timestamps, or evidence.
- Do not claim a root cause is confirmed without supporting evidence.
- Require explicit human approval before consequential production actions.
- Record approved, rejected, and pending decisions separately.
- Treat rollback, retries, DLQ replay, refunds, configuration changes, and customer communications as actions that may require separate approval.
- Label simulations clearly and never describe simulated actions as completed production changes.

These are workflow guardrails demonstrated in the agent instructions; they are not a substitute for production access controls, monitoring, or operational policy.

## Demonstration: INC-001 — UPI payment gateway incident

A simulated incident was used to test the collaboration workflow.

**Scenario:** Payments were reported as failing or remaining pending after a recent payment-service deployment. No real logs, metrics, deployment diff, or production access were supplied.

The Investigator ranked several possible explanations, including a deployment regression, callback or webhook handling, connection-pool or infrastructure exhaustion, and an upstream PSP/bank issue. The Reviewer challenged the assumption that deployment timing proved causation and highlighted risks around retries, idempotency, callback verification, refunds, and reconciliation.

The team prepared a rollback plan but did not execute it. The simulation recorded a human approval to freeze further deployments, while other operational actions remained unapproved. The final review accepted the investigation plan with conditions, not a confirmed diagnosis.

**Outcome:** The incident remained open in the simulation, the root cause remained unconfirmed, and no production systems were changed.

## Getting started

This repository is a project overview and workflow record. The current setup was performed through the Switch interface; a portable, scripted installation is not included here.

To explore or reproduce the workflow:

1. Review the [Switch documentation](https://docs.switchagents.ai/llms.txt) and the custom-agent setup guidance.
2. Set up Switch and configure a room for the incident-response workflow.
3. Create the four agents listed in the architecture section, using their responsibilities as the basis for their instructions.
4. Configure the agents to run locally with Claude Code, each with its own working directory.
5. Connect a Telegram bot and group to the room, following Switch's current documentation. Keep bot tokens and credentials private.
6. Run a clearly labelled simulated incident first. Verify that agents distinguish facts from hypotheses and that no consequential action is taken without human approval.

Exact setup screens and supported configuration options may vary by Switch version.

## Screenshots

<img width="1273" height="555" alt="SwitchOps command center screenshot" src="https://github.com/user-attachments/assets/8e743138-198b-4381-ac22-25a3c727c77e" />
<img width="1266" height="555" alt="SwitchOps agent workflow screenshot" src="https://github.com/user-attachments/assets/a6a87501-97f4-4770-bf32-028992bf4bbd" />

## Roadmap

- [ ] Export and publish a sanitized, reproducible agent configuration.
- [ ] Add example agent instruction files.
- [ ] Add redacted screenshots and a short end-to-end demo.
- [ ] Define reusable incident templates and severity criteria.
- [ ] Add structured incident data and report versioning.
- [ ] Add automated tests for approval boundaries and fact-versus-hypothesis handling.

## Contributing

Suggestions and improvements are welcome. Please avoid submitting secrets, personal data, real customer information, production logs, or unredacted incident details.

For substantial changes, open an issue describing the proposed improvement before submitting a pull request.

## License

No license has been selected yet. Until a license is added, the repository should be treated as having all rights reserved; do not assume that reuse, modification, or redistribution is permitted.
