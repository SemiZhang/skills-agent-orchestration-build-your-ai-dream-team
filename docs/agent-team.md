# Agent team

For Mona's Project Pulse dashboard, we will use a four-agent custom team orchestrated through GitHub Copilot CLI in a Codespace. The team is defined under the repository's agent folder and each specialist has a clear scope so the work can be planned, designed, implemented, and integrated without overlap.

## Planner

- Agent: Planner
- Target model: Claude Opus 4.7 (copilot)
- Responsibility: Research the codebase and requirements, identify dependencies and edge cases, and produce an ordered implementation plan with file assignments, validation expectations, and open questions.
- Definition: `.github/agents/planner.agent.md`

## Orchestrator

- Agent: Orchestrator
- Target model: Claude Opus 4.7 (copilot)
- Responsibility: Coordinate the specialist agents, break the work into phases, assign file scopes, run tasks in parallel when safe, and verify that the integrated result makes sense before reporting back.
- Definition: `.github/agents/orchestrator.agent.md`

## Designer

- Agent: Designer
- Target model: Gemini 3.1 Pro (copilot)
- Responsibility: Focus on UI/UX, information hierarchy, accessibility, responsive behavior, and dashboard styling so the Project Pulse experience feels polished and easy to use.
- Definition: `.github/agents/designer.agent.md`

## Coder

- Agent: Coder
- Target model: GPT-5.5 (copilot)
- Responsibility: Implement the application logic, fix bugs, and write or adjust code within the file scope assigned by the Orchestrator while validating the behavior before completion.
- Definition: `.github/agents/coder.agent.md`

## Working model

This project will be orchestrated with GitHub Copilot CLI in a Codespace, using the Planner to determine the path forward, the Orchestrator to delegate tasks, the Designer to shape the dashboard experience, and the Coder to build the working implementation.
