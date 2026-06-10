# Project Pulse — Implementation Plan

Summary
- Goal: Add a lightweight, deterministic "Project Pulse" dashboard to the repo that demonstrates a polished static frontend the learner can open in the Codespace. The dashboard will be a single-page static app under app/, driven by a small JSON data file and styled with an explicit CSS surface. It should be visually recognizable as a dashboard (cards, status badges, priorities), accessible, responsive, and easy to validate.
- Primary deliverables:
  - app/index.html — static HTML UI and small client-side script to load project-data.json
  - app/styles.css — Designer-owned styling implementing .dashboard and .project-card hooks
  - app/project-data.json — sample data the UI consumes
  - .vscode/launch.json — deterministic launch configuration (cwd `${workspaceFolder}/app`, open index.html)

Phases (ordered) with estimated effort
1. Project scaffolding and data (Coder) — 1.0–1.5 hours
   - Create app/index.html skeleton and minimal JS to fetch app/project-data.json.
   - Create sample app/project-data.json with 6 project entries covering statuses, priorities, owners.
2. Visual design & CSS (Designer) — 1.5–2.5 hours
   - Create app/styles.css implementing the visual system, responsive grid, .dashboard, .project-card, .status-badge, and priority treatments.
3. VS Code preview config (Coder) — 0.25–0.5 hours
   - Add .vscode/launch.json with deterministic JSON settings to open index.html and set cwd.
4. Integration polish & accessibility checks (Joint: Designer + Coder) — 0.75–1.5 hours
   - Minor tweaks in markup/CSS for contrast, aria attributes, keyboard focus states, and verify data rendering.
5. Validation & documentation (Planner/Orchestrator) — 0.5–1 hour
   - Add docs/project-pulse-plan.md (this file) and basic validation instructions.

File assignments (explicit)
- app/index.html (Coder)
  - Create a single static HTML file.
  - Responsibilities:
    - Static semantic structure: header with title, a container element with class="dashboard" to hold project cards, and minimal inline client script or linked script that fetches app/project-data.json and renders cards into .dashboard.
    - Provide clear CSS hooks required by Designer: .dashboard, .project-card, .project-title, .project-meta, .status-badge, .priority-high/.priority-medium/.priority-low.
    - Add basic aria landmarks (role="main") and data-test attributes to aid tests.
    - Keep interactive JS minimal and deterministic (no frameworks).
    - Do not touch styling other than adding class names and a single <link rel="stylesheet" href="styles.css"> reference.
    - Scope small enough so Designer can style without changing HTML semantics.

- app/styles.css (Designer)
  - Create the complete visual style for the dashboard.
  - Responsibilities:
    - Implement layout (responsive CSS grid), card styles, status badges, priority color accents, readable typography, shadows, spacing.
    - Provide deterministic hooks in selectors for all classes listed above.
    - Include focus, hover, and reduced motion support.
    - Make first view polished: visible cards, good contrast, mobile-friendly.
    - Keep CSS scoped to selectors; avoid changing HTML/JS behavior.

- app/project-data.json (Coder)
  - Create a deterministic sample dataset (UTF-8 JSON array) with about 6 project objects:
    - Fields: id, name, owner, status (one of "on-track", "at-risk", "off-track"), priority ("high"|"medium"|"low"), progress (0-100), shortDescription.
  - Include at least one long title and one long description to test truncation/responsive behavior.
  - No comments, plain JSON parsable with fetch.

- .vscode/launch.json (Coder)
  - Create strict JSON (no comments) in .vscode/launch.json.
  - Must set:
    - cwd: "${workspaceFolder}/app"
    - Request/launch settings to open index.html in the Codespace preview (use a deterministic Live Server or simple "open file" config per repository patterns).
  - If the repo uses a known extension in other exercises, match that pattern; otherwise provide a simple "open in browser preview" configuration that is deterministic and minimal.

Dependencies between steps and external dependencies
- Step 1 (Scaffold & data) → Step 2 (CSS): HTML and JSON must exist before Designer styles page, because Designer will reference the markup hooks. However, Designer can start drafting CSS in parallel if the markup contract (class names) is agreed up front. To avoid overlapping edits, the Orchestrator assigns class-name contract in Step 1.
- Step 3 (.vscode/launch.json) depends on app/index.html existing so the preview target is valid.
- Step 4 (Accessibility and polish) requires Steps 1–3 complete.
- External dependencies: none (static files only). Use only standards-compliant HTML/CSS/vanilla JS; no external CDN dependencies to keep app deterministic.

Parallelizable work vs. sequential
- Parallelizable:
  - Designer can start crafting styles.css in parallel with Coder creating index.html if they agree on an explicit, small class-name contract up front. Rationale: CSS and HTML are independent if class hooks are agreed; reduces blocked time.
  - While Coder writes project-data.json, Designer can prepare styles for placeholder content (e.g., using sample class names).
- Sequential:
  - Adding .vscode/launch.json must wait for index.html to exist so the target is valid.
  - Final accessibility polish must be sequential after both markup and styling exist to validate real rendering.

Edge cases and risks (with mitigations)
- Empty or malformed JSON:
  - Risk: fetch fails or UI shows nothing.
  - Mitigation: index.html should include simple error handling and a friendly "No project data" message. project-data.json must be valid JSON.
- Long titles/descriptions overflow:
  - Mitigation: Designer implements truncation (line-clamp) and tooltip/aria-label to show full text.
- Color contrast/accessibility:
  - Mitigation: Use accessible contrast colors for text on badges; test with simple contrast checks (WCAG 4.5:1 or higher for UI elements).
- Workspace differences (Codespace vs. local):
  - Mitigation: .vscode/launch.json uses ${workspaceFolder}/app and opens index.html directly — deterministic.
- Agent overlap risk:
  - Mitigation: Keep assignments strict: Designer only touches app/styles.css; Coder only touches index.html, project-data.json, .vscode/launch.json.

Validation expectations (how to validate each phase)
1. Scaffolding & Data (Coder)
   - Manual checks:
     - Open app/project-data.json in editor to ensure valid JSON.
     - Open app/index.html in browser (or preview) and confirm page shows a meaningful placeholder or renders cards if script loads JSON.
   - Tests:
     - Confirm fetch returns data in console (simple console.log).
2. Styles (Designer)
   - Open index.html in browser and confirm:
     - .dashboard contains cards.
     - Cards render with visible status badges and priority treatments.
     - Responsive behavior: shrink window to mobile width and ensure grid collapses.
     - Keyboard navigation: tabbing focuses interactive elements.
3. Launch config (Coder)
   - Open VS Code Run/Debug or the configured preview; ensure index.html opens and the page loads from cwd.
4. Integration polish
   - Validate error handling: rename project-data.json temporarily to ensure "No data" message appears.
   - Validate accessibility: check aria labels, contrast, and reduced-motion preference.
5. Completion
   - Final walkthrough: open app/index.html in Codespace preview and visually confirm 6 cards render with varied statuses.

Open questions and suggested mitigations
- Which exact preview launch flavor does the repository prefer (Live Server, browser preview, or opening file)? 
  - Suggestion: Use a simple "open index.html" launch that works in Codespaces; if repository elsewhere uses Live Server, adapt to that pattern.
- Should the dashboard include sorting/filtering interactions?
  - Suggestion: Keep initial scope read-only. Add filters in a follow-up phase if requested.
- Any preferred color palette or brand tokens?
  - Suggestion: Designer chooses accessible, neutral palette and documents choices in a short comment in app/styles.css.

Notes on avoiding merge conflicts
- Keep file scopes strict:
  - Designer: ONLY modify app/styles.css.
  - Coder: ONLY modify app/index.html, app/project-data.json, .vscode/launch.json.
- Orchestrator to ensure each agent is assigned non-overlapping files during implementation.

Validation checklist for the Orchestrator (delegateable)
- [ ] Coder: Create app/index.html with semantic markup, script to fetch project-data.json, and class hooks (.dashboard, .project-card, etc.)
- [ ] Coder: Create app/project-data.json with 6 sample projects (on-track/at-risk/off-track, priorities).
- [ ] Designer: Create app/styles.css implementing responsive grid, .project-card, .status-badge, and priority classes.
- [ ] Coder: Add .vscode/launch.json (strict JSON) with cwd "${workspaceFolder}/app" and open index.html.
- [ ] Joint: Run integration checks: open preview, validate data loads, test error state and accessibility basics.
- [ ] Planner/Orchestrator: Add this plan to docs/project-pulse-plan.md and confirm all validation steps completed.

This plan is intentionally small-scope and deterministic so Designer and Coder can work in parallel where safe and avoid file conflicts.
