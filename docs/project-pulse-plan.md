# Project Pulse implementation plan

## Goal and repository context

Build a lightweight static Project Pulse dashboard for contributors, with project ownership, status, recent activity, priority or risk, and a short contributor-friendly summary. The brief in `.github/project-pulse-brief.md` specifies the app files and the VS Code launch configuration. The Step 3 instructions and `.github/workflows/3-step.yml` supply concrete content checks and launch requirements.

The `app/` directory and `.vscode/launch.json` do not exist yet. The repository has no frontend build setup; keep the dashboard static and runnable with Python's built-in HTTP server. Match the existing VS Code configuration style in `.vscode/tasks.json`, while keeping `launch.json` strict JSON without comments as required by the exercise.

## Responsibilities and file assignments

| Owner | File(s) | Responsibility |
|---|---|---|
| Designer | Design guidance for `app/index.html` and `app/styles.css` | Define information hierarchy, card layout, status and priority treatments, responsive behavior, readable spacing, and accessibility/contrast requirements. Agree on the selectors and structure Coder will implement; avoid editing Coder-owned files. |
| Coder | `app/index.html` | Implement semantic dashboard markup, the exact “Project Pulse” title, styles/data references, and visible project cards that display required data fields. Keep any necessary rendering logic inline; no extra application files are in scope. |
| Coder | `app/styles.css` | Implement the agreed visual design, responsive layout, accessible status/priority cues, and required `.dashboard`, `.project-card`, `border-radius`, and `box-shadow` styling. |
| Coder | `app/project-data.json` | Provide representative sample data under a top-level `projects` array. Each project includes `name`, `owner`, `status`, `recentActivity`, and `priority`; a short contributor-friendly summary may be included as an additional field. |
| Coder | `.vscode/launch.json` | Add the `Run Project Pulse Dashboard` launch configuration using `python3 -m http.server 5500`, with `cwd` set to `${workspaceFolder}/app` and `serverReadyAction` opening `http://localhost:%s/index.html`. Use strict JSON. |
| Orchestrator | All four implementation files | Coordinate the handoff, check scope and integration, and ensure validation is completed. The learner retains all staging, commit, and push operations. |

## Phases and dependencies

1. **Confirm the contract.** Use the brief, Step 3 instructions, and workflow checks as acceptance criteria. This is the prerequisite for design and implementation.
2. **Define the design and data contract.** Designer specifies the card structure, visual hierarchy, accessible treatments, responsive behavior, and class/selector contract. Coder agrees on the project field names and data shape from the brief.
3. **Prepare independent support files.** Once the contract is fixed, Coder creates `app/project-data.json` and `.vscode/launch.json`. These files are independent of one another and do not need final CSS.
4. **Implement the UI.** With the design/selector contract and data shape established, Coder implements `app/index.html`, then styles the stable markup in `app/styles.css`. The HTML must reference `styles.css` and `project-data.json` and render cards populated from the projects data.
5. **Integrate and validate.** Review all four files together, run the checks below, and launch the dashboard to verify the actual preview.

Dependencies:

- Design guidance and the shared field/selector contract precede final HTML and CSS work.
- The JSON data shape precedes HTML rendering from that data.
- CSS implementation follows the HTML structure so selectors match.
- The launch configuration can be prepared independently once the app path and preview URL are agreed.
- Integrated validation follows completion of all assigned files.

## Parallel-work decisions

- **Parallelize:** Designer can develop design guidance while Coder prepares `app/project-data.json` and `.vscode/launch.json` after the acceptance contract is confirmed. The JSON and launch configuration can also be created in parallel because they do not share files or depend on final markup or styling.
- **Keep sequential:** Finalizing `app/index.html` and `app/styles.css` should be sequential under Coder. Designer provides the visual and accessibility direction first; Coder implements markup, then styles it. This avoids overlapping edits and prevents mismatched class names or layout assumptions.
- **Do not split file ownership:** Coder owns all four implementation files. Designer owns the design decisions and review recommendations, not concurrent edits to Coder's files. This keeps the handoff clear while using Designer's specialist input.
- **Do not parallelize final validation:** A single integrated validation pass should assess the completed files and launched app together.

## Validation expectations

### Automated acceptance checks

Run the equivalent checks defined in `.github/workflows/3-step.yml`:

- Confirm `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Confirm `app/index.html` includes `Project Pulse`, references `styles.css` and `project-data.json`, and renders `.project-card` elements with visible `status`, `recentActivity`, and `priority` values.
- Confirm `app/styles.css` includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Parse both JSON files, for example with `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`.
- Confirm project data has a top-level `projects` array and every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm the launch configuration is named `Run Project Pulse Dashboard`, serves from `app/`, and opens `index.html` via `http://localhost:%s/index.html`.

### Manual preview and quality checks

- Select **Run Project Pulse Dashboard** in VS Code Run and Debug and verify the browser opens the dashboard, not a directory listing.
- Verify multiple project cards render and each required field is readable.
- Check desktop and narrow viewport layouts, long names/activity text, keyboard/screen-reader-friendly semantic structure, and sufficient contrast. Status and priority must have readable text cues and not rely on color alone.
- Confirm the dashboard works without a build step or added dependencies.

## Assumptions

- The implementation remains a static app with no framework, package manifest, or additional application files.
- The supported preview path is the VS Code launch configuration running the server from `app/`; opening the HTML directly as a `file://` URL is not required for fetching JSON.
- The brief's short contributor-friendly summary is an optional additional project field or supporting UI copy; it must not replace any required fields.
