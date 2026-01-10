---
name: multimodal-ui-flow-analyzer
description: Analyze annotated UI screenshots and markdown documentation to generate agent-consumable UI flow specifications. Use when processing web app UI flows described via markdown + screenshots into structured, automation-ready knowledge.
license: MIT
metadata:
  author: Bowen
  version: "1.0"
  tags:
    - ui-analysis
    - multimodal
    - automation
    - workflow
---

# Multimodal UI Flow Analyzer

This skill enables you to analyze static web app UI flows described via **markdown + annotated screenshots** and output **agent-consumable knowledge** for downstream AI code agents.

---

## When to Use This Skill

Activate this skill when:
- Processing UI documentation that includes annotated screenshots
- Converting visual UI flows into structured automation specs
- Generating test automation guidance from UI walkthroughs
- Creating machine-readable UI interaction sequences

---

## Core Workflow (7 Steps)

### Step 1: Normalize Input Markdown

Before analysis, ensure each UI step follows this structure:

```md
## Step N: <Short Title>

**Intent:**
What the user is trying to accomplish.

**User Action (Text):**
Plain-language description of the interaction.

**Visual Reference:**
![step-n](path/to/image.png)

**Visual Annotations:**
- Box / arrow / highlight descriptions
```

If the input doesn't follow this format, restructure it first.

---

### Step 2: Apply Vision-Aware Analysis Rules

When analyzing screenshots:

1. **Identify interactive UI elements** (buttons, inputs, menus, links)
2. **Map visual annotations** (boxes, arrows, highlights) to UI elements
3. **Infer user intent** from both text and visual cues
4. **Ignore decorative elements** that are non-interactive
5. **Assume static UI** (no animations or runtime state changes)

---

### Step 3: Process Each Step Atomically

Analyze **one step at a time**, never the entire document at once.

For each step, extract:
- The UI element being interacted with
- Its visual characteristics and location
- Its technical role in the web application
- Preconditions and resulting state

---

### Step 4: Treat Annotations as Ground Truth

**Annotation Priority Rules:**
- Highlighted areas are authoritative targets
- Prefer annotated elements over textual ambiguity
- If text and image conflict, **image evidence wins**

Map annotations explicitly:
```json
{
  "annotation_mapping": {
    "red_box": "Primary action button",
    "arrow": "Cursor movement direction",
    "highlight": "Target input field"
  }
}
```

---

### Step 5: Generate Structured Output

Produce output in the canonical format (see templates in `assets/templates/`).

**Per-Step JSON Format:**

```json
{
  "step_id": "step-N",
  "intent": "Description of user goal",
  "action": "click|type|select|scroll|hover",
  "ui_element": {
    "type": "button|input|link|menu|dropdown",
    "label": "Visible text or aria-label",
    "visual_location": "Position description",
    "identification_strategy": [
      "visible text equals 'X'",
      "role=button",
      "data-testid='element-id'"
    ]
  },
  "precondition": "Required state before action",
  "resulting_state": "Expected state after action"
}
```

**Flow Markdown Format:**

```md
# UI_FLOW: <flow_name>

## Metadata
- App: <Application Name>
- Flow Type: Static UI Interaction
- Source: Annotated screenshots + human-authored text

---

## Step 1
**Intent:** <goal>

**Action:**
- type: <action_type>
- target:
  - role: <element_role>
  - text: "<visible_text>"
  - location: <position_description>

**Preconditions:**
- <required_state>

**Postconditions:**
- <resulting_state>

**Automation Notes:**
- <selector_recommendations>
```

---

### Step 6: Add Automation Hints

For each step, include:

1. **Stable DOM selectors** (prefer semantic)
   - `role=button` + visible text
   - `data-testid` attributes
   - `aria-label` values

2. **Brittle selectors to avoid**
   - Pixel-based positions
   - Absolute CSS selectors
   - Dynamic class names

3. **Wait conditions**
   - Elements to wait for before action
   - Loading states to handle

---

### Step 7: Validate the Output

Before finalizing, verify:

- [ ] All steps have clear preconditions
- [ ] Step ordering is logical and complete
- [ ] No ambiguous UI references remain
- [ ] Each action has a defined resulting state
- [ ] Selectors are stable and semantic where possible

---

## Output Templates

Use templates from `assets/templates/`:

| Template | Purpose |
|----------|---------|
| `step-output.json` | Single step structured output |
| `flow-output.md` | Complete flow specification |
| `automation-hints.md` | Test automation guidance |

---

## Example Interaction

**Input:** User provides markdown with annotated screenshot showing a "Create Project" button highlighted with a red box.

**Analysis Process:**
1. Parse step structure from markdown
2. Identify red box annotation → maps to button element
3. Extract button text: "Create Project"
4. Determine location: "top-right of main content area"
5. Infer action type: click
6. Define precondition: "User is on Projects dashboard"
7. Define postcondition: "Project creation modal opens"

**Output:**
```json
{
  "step_id": "step-2",
  "intent": "Create a new project",
  "action": "click",
  "ui_element": {
    "type": "button",
    "label": "Create Project",
    "visual_location": "top-right of main content area",
    "identification_strategy": [
      "visible text equals 'Create Project'",
      "role=button"
    ]
  },
  "precondition": "User is on Projects dashboard",
  "resulting_state": "Project creation modal opens"
}
```

---

## Edge Cases

### Ambiguous Annotations
If multiple elements are highlighted, process them in visual reading order (top-to-bottom, left-to-right).

### Missing Screenshots
If a step lacks a visual reference, flag it and proceed with text-only analysis. Note reduced confidence in output.

### Complex Multi-Element Interactions
For drag-and-drop or multi-select, describe both source and target elements with separate identification strategies.

### Dynamic Content
If the UI shows dynamic content (lists, tables), describe the interaction pattern rather than specific instances.

---

## Constraints

- **DO NOT** assume backend logic or API behavior
- **DO NOT** infer state beyond what's visible
- **DO NOT** generate pixel coordinates as primary selectors
- **ALWAYS** prefer semantic selectors over structural ones
- **ALWAYS** document uncertainty when present
