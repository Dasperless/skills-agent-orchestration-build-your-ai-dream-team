# Agent team

Mona's Project Pulse dashboard will be built by a custom agent team orchestrated through the GitHub Copilot CLI in a Codespace. The Orchestrator coordinates the workflow, using the Planner to establish an implementation approach before assigning non-overlapping code and design work to the specialists.

| Agent | Target model | Responsibility | Definition |
|---|---|---|---|
| Orchestrator | Claude Opus 4.7 (copilot) | Breaks work into phases, delegates explicit file scopes to specialists, coordinates dependencies, verifies the integrated result, and reports the outcome. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies dependencies and risks, and produces an ordered implementation plan with file assignments and validation expectations. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements application logic and bug fixes within its assigned scope, creates assigned runnable-app support when needed, and validates its changes. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Owns the dashboard's UI/UX, accessibility, information architecture, interaction flow, responsive behavior, and polished Project Pulse visual design. | `.github/agents/designer.agent.md` |

All team agents leave staging, commits, and pushes under the learner's control through Copilot CLI prompts.
