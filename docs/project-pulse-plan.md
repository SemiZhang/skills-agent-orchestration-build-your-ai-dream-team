# Project Pulse Dashboard Implementation Plan

## Goal and repository context

Build Mona's Project Pulse as a small, static dashboard that helps contributors see multiple projects, their owners, status, recent activity, and priority or risk, with a short contributor-friendly summary and a polished, accessible card-based layout.

The brief is in `.github/project-pulse-brief.md`; implementation requirements and checks are in `.github/steps/3-step.md` and `.github/workflows/3-step.yml`. Step 2 validates this plan at `docs/project-pulse-plan.md` and checks for project context, Designer and Coder responsibilities, all four required file assignments, dependencies, parallel work, and validation. The repository has no app implementation yet; `.vscode/tasks.json` exists, but `.vscode/launch.json` does not. Keep the dashboard framework-free and static.

## Responsibilities

- **Planner:** Research requirements and repository conventions, define phases, dependencies, file ownership, edge cases, and validation expectations. Keep the plan aligned with the brief and automated checks.
- **Orchestrator:** Assign non-overlapping work, share the data and markup contracts, decide what can safely run in parallel, integrate the results, and ensure the final validation is completed.
- **Designer:** Define the information hierarchy, project-card content order, visual styling direction, responsive behavior, and accessibility expectations. Hand off design tokens and markup/CSS hooks to the Coder; avoid simultaneous edits to implementation files.
- **Coder:** Implement the assigned static files and VS Code launch configuration, render project data, handle empty and error cases, and run the agreed checks. Coordinate shared contracts with the Designer before implementation.

## File assignments and phases

| Phase | File(s) | Owner | Work |
|---|---|---|---|
| 1. Agree on contracts | None | Planner, Designer, Coder; Orchestrator decides | Confirm data fields, project-card hierarchy, status/priority vocabulary, CSS hooks, and preview behavior before parallel implementation. |
| 2. Create project data | `app/project-data.json` | Coder | Add a top-level `projects` array with multiple entries. Every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a concise contributor-friendly `summary` field to satisfy the brief's summary requirement. |
| 3. Build the page and renderer | `app/index.html` | Coder | Use the exact title `Project Pulse`; reference `styles.css` and `project-data.json`; render a `.project-card` per data entry and show all required fields and summary using accessible, semantic markup. Include meaningful loading, empty, and fetch/parse-error feedback. |
| 4. Style the dashboard | `app/styles.css` | Designer defines direction; Coder implements | Include `.dashboard` and `.project-card`, a clear hierarchy, responsive card layout, readable spacing, `border-radius`, and `box-shadow`. Provide visible keyboard focus and sufficient contrast; never communicate status or priority by color alone. |
| 5. Configure preview | `.vscode/launch.json` | Coder | Create strict JSON with a configuration named `Run Project Pulse Dashboard`. Serve from `app/` using `python3 -m http.server 5500`, and open `http://localhost:%s/index.html` so the app opens rather than a directory listing. Set the working directory to the app directory. Use a launch configuration type supported by the Codespace and compatible with launching the server and its ready action. |
| 6. Integrate and verify | All four implementation files | Coder and Designer; Orchestrator coordinates | Test the data, rendering, responsive presentation, accessibility basics, and actual VS Code launch flow against the expectations below. Resolve integration defects within the assigned file scopes. |

## Dependencies and parallel work decisions

- The required data fields, optional `summary` field, and card/CSS hooks must be agreed in Phase 1. This contract is a prerequisite for parallel implementation and prevents the renderer, data, and stylesheet from drifting apart.
- After Phase 1, the Coder can prepare `app/project-data.json` and the structural parts of `app/index.html` in parallel, provided the agreed schema is used. The Coder can also prepare `.vscode/launch.json` in parallel after confirming the supported launch configuration type and the required command, working directory, and URL.
- The Designer can develop the visual direction and accessibility decisions in parallel with the Coder's data and page scaffolding. Once the Designer and Coder agree on selectors and hooks, the Coder can implement `app/styles.css` without conflicting edits.
- Final integration depends on all three app files and the launch configuration being present. Runtime and visual validation must follow integration; checks on partial files do not establish that the dashboard works end to end.
- The Orchestrator should keep file ownership exclusive during implementation: Designer owns design decisions, while Coder owns edits to all four required implementation files. If the Designer is explicitly assigned stylesheet edits instead, the Orchestrator must update ownership before work begins.

## Risks and edge cases

- Browsers may block fetching JSON when the HTML is opened through `file://`; preview through the configured local HTTP server.
- Empty, invalid, or unavailable project data should produce a useful visible message rather than a blank dashboard.
- Long project names, summaries, or activity descriptions should wrap without breaking the cards or causing horizontal overflow.
- Status and priority labels must remain understandable to assistive technology and when color is unavailable.
- The Codespace may not have a Python debug provider installed. Confirm the launch configuration type works in the actual environment; syntactically valid JSON alone does not prove the launch action works.
- Port 5500 may already be in use. Report the conflict and stop the existing preview process or coordinate a port change rather than silently making the launch URL inconsistent.
- The Step 3 workflow checks file presence, required phrases, and JSON syntax. They do not test the rendered UI, accessibility, or a real launch.

## Validation expectations

1. Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
2. Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`; both must parse successfully.
3. Confirm the HTML has the exact `Project Pulse` title, references `styles.css` and `project-data.json`, renders `.project-card` elements from the `projects` array, and displays each project's status, recent activity, priority, and summary.
4. Confirm the CSS includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, plus responsive layout, visible focus states, and readable contrast.
5. Confirm the JSON has a top-level `projects` array and every entry includes `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
6. Compare the implementation against `.github/workflows/3-step.yml` required paths and keyphrases. Passing these automated checks is necessary but not sufficient.
7. Run **Run Project Pulse Dashboard** from VS Code. Verify the server starts from `app/`, the browser opens `http://localhost:<port>/index.html`, and project cards—not a directory listing—are displayed.
8. Check a narrow viewport, keyboard navigation and visible focus, and the textual status and priority labels. If practical, verify the empty or invalid data state.
9. Stop the preview server after testing. Report only checks actually run; the repository does not define a dedicated dashboard test suite.
