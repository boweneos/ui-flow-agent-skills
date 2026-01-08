# Multimodal UI Flow Understanding Guide (LLM + Vision)

This document consolidates a **7-step, end-to-end methodology** for teaching an LLM with vision capabilities (e.g. Gemini Pro 3) to understand **static web app UI flows** described via **markdown + annotated screenshots**, and to output **agent-consumable knowledge** for downstream AI code agents.

---

## Step 1: Normalize the Input Markdown (Source of Truth)

Before involving any LLM, standardize how UI steps are written.

### Required Structure Per Step

```md
## Step N: <Short Title>

**Intent:**
What the user is trying to accomplish.

**User Action (Text):**
Plain-language description of the interaction.

**Visual Reference:**
![step-n](https://s3/.../image.png)

**Visual Annotations:**
- Box / arrow / highlight descriptions
```

### Why This Matters
- Enables deterministic multimodal grounding
- Avoids free-form ambiguity
- Allows step-by-step reasoning and parsing

---

## Step 2: Define Vision-Aware System Instructions

Provide explicit perception rules so the LLM understands how to interpret UI screenshots.

### Recommended System Prompt

```text
You are a UI interaction analyst.

When images are provided:
- Identify interactive UI elements (buttons, inputs, menus)
- Map visual annotations (boxes, arrows, highlights) to UI elements
- Infer user intent from both text and visual cues
- Ignore decorative or non-interactive elements
- Assume static UI (no animations or runtime state changes)
```

This prevents hallucination and focuses the model on actionable UI affordances.

---

## Step 3: Parse Each Step as an Atomic Multimodal Unit

Process **one step at a time**, never the entire document at once.

### Step-Level Prompt Template

```text
Given:
1. A single UI step description
2. An annotated UI screenshot

Tasks:
- Identify the UI element being interacted with
- Describe its visual characteristics
- Infer its technical role in a web application
- Output a structured representation

Constraints:
- Use only visible or explicitly described information
- Do not assume backend logic
- Be explicit and deterministic
```

### Expected Intermediate Output (JSON)

```json
{
  "step_id": "step-2",
  "intent": "Create a new project",
  "action": "click",
  "ui_element": {
    "type": "button",
    "label": "New Project",
    "visual_location": "top-right of main content area",
    "identification_strategy": [
      "visible text equals 'New Project'",
      "role=button"
    ]
  },
  "precondition": "User is on Projects dashboard",
  "resulting_state": "Project creation modal opens"
}
```

This JSON is the bridge between perception and code generation.

---

## Step 4: Treat Visual Annotations as Ground Truth

Explicitly instruct the model how to use annotations.

### Annotation Handling Rules

```text
If annotations exist:
- Treat highlighted areas as authoritative targets
- Prefer annotated elements over textual ambiguity
- If text and image conflict, image evidence wins
```

### Annotation Mapping Example

```json
"annotation_mapping": {
  "red_box": "Primary action button",
  "arrow": "Cursor movement direction"
}
```

Annotations dramatically reduce ambiguity and hallucination.

---

## Step 5: Generate Canonical, Agent-Ready Flow Markdown

Convert parsed steps into a **machine-consumable UI flow specification**.

### Recommended Output Format

```md
# UI_FLOW: create_project

## Metadata
- App: Web App
- Flow Type: Static UI Interaction
- Source: Annotated screenshots + human-authored text

---

## Step 1
**Intent:** Navigate to projects dashboard

**Action:**
- type: click
- target:
  - role: navigation_button
  - text: "Projects"
  - location: top navigation bar

**Preconditions:**
- User is authenticated

**Postconditions:**
- Projects dashboard is visible
```

This format is readable by humans and deterministic for agents.

---

## Step 6: Add Implementation & Automation Hints

Ask the LLM to think like a code agent.

### Additional Instructions

```text
For each step:
- Suggest stable DOM selectors where possible
- Identify brittle selectors (e.g., pixel-based, absolute positions)
- Prefer semantic roles and visible text
```

### Example Augmentation

```md
**Automation Notes:**
- Prefer role=button + visible text selector
- Avoid absolute positioning or CSS-only selectors
```

This bridges UI intent → test automation → code changes.

---

## Step 7: Run a Validation & Consistency Pass

Perform a final LLM review over the generated flow.

### Validation Prompt

```text
Review this UI flow and:
- Check for missing preconditions
- Validate step ordering
- Flag ambiguous or unstable UI references
- Ensure each action has a clear resulting state
```

This ensures production-grade flow documentation.

---

## Final Mental Model

You are not training the LLM.
You are constraining it.

- Structured input beats prose
- Images ground intent
- Annotations eliminate ambiguity
- Canonical schemas unlock automation

This approach scales from documentation to **AI-driven UI code generation**.
