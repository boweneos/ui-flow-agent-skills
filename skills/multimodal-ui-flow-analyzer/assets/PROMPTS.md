# Prompt Templates for Multimodal UI Flow Analyzer

Copy and customize these prompts to trigger the skill effectively.

---

## 🎯 Quick Trigger Prompts

### Analyze Single UI Step

```
Analyze this UI step and output structured JSON:

## Step [N]: [Title]

**Intent:** [What the user wants to accomplish]

**User Action:** [Description of interaction]

**Visual Reference:** [screenshot]

**Visual Annotations:**
- [annotation descriptions]
```

### Analyze Complete Flow

```
Using the multimodal-ui-flow-analyzer skill, analyze this complete UI flow 
and generate a structured specification with automation hints:

[paste full flow documentation]
```

### Quick Screenshot Analysis

```
Analyze this annotated screenshot. Identify:
1. The primary action target (highlighted element)
2. Recommended selectors for automation
3. Preconditions and expected results

[attach screenshot]
```

---

## 📋 Detailed Analysis Prompts

### Full Flow Conversion

```
Convert this UI walkthrough into an agent-consumable specification.

Requirements:
- Follow the canonical flow output format
- Include automation hints for each step
- Flag any ambiguous UI references
- Suggest stable DOM selectors

Input Documentation:
---
[paste your markdown documentation here]
---
```

### Generate Test Automation

```
For the following UI flow, generate:
1. Playwright test code skeleton
2. Selector strategy recommendations
3. Wait condition suggestions
4. Edge case handling notes

Flow:
---
[paste flow specification]
---
```

### Validate Existing Flow

```
Review this UI flow specification and:
- Check for missing preconditions
- Validate step ordering is logical
- Flag ambiguous or unstable UI references
- Ensure each action has a clear resulting state
- Suggest improvements

Flow to validate:
---
[paste flow specification]
---
```

---

## 🔄 Batch Processing Prompts

### Process Multiple Flows

```
Analyze all UI flow documents in the [folder] directory.

For each file:
1. Parse the UI steps
2. Generate structured JSON output
3. Create automation hints
4. Save output to [output_folder]/[filename]-spec.json

Report any files that don't follow the expected format.
```

### Compare Flows

```
Compare these two UI flows and identify:
- Common steps that could be shared
- Differences in interaction patterns
- Inconsistencies in naming or structure

Flow A:
---
[paste flow A]
---

Flow B:
---
[paste flow B]
---
```

---

## 🖼️ Screenshot-Focused Prompts

### Annotation Interpretation

```
This screenshot has the following annotations:
- Red box around [element]
- Arrow pointing to [target]
- Highlight on [area]

Interpret these annotations and output:
1. The intended user action
2. Target element identification strategies
3. Expected result of the action
```

### Multi-Screenshot Flow

```
I have [N] screenshots showing a complete user flow.

Screenshot 1: [description or attach]
Screenshot 2: [description or attach]
...

Analyze the sequence and generate:
1. Step-by-step flow specification
2. Transition conditions between steps
3. Complete automation script
```

---

## 🛠️ Specialized Prompts

### Accessibility-Focused Analysis

```
Analyze this UI flow with accessibility in mind:
- Verify all elements have proper ARIA roles
- Check for keyboard navigation support
- Suggest accessible selectors
- Flag potential accessibility issues

Flow:
---
[paste flow]
---
```

### Mobile-Responsive Analysis

```
This UI flow is for a responsive web app. Analyze and provide:
- Desktop interaction sequence
- Mobile-specific considerations
- Touch vs click action differences
- Viewport-dependent element locations

Flow:
---
[paste flow]
---
```

### Form Interaction Analysis

```
Analyze this form submission flow:
- Identify all form fields and their types
- Map validation requirements
- Suggest fill order for automation
- Handle error state scenarios

Form Flow:
---
[paste flow]
---
```

---

## 📊 Output Format Requests

### Request JSON Output

```
Analyze this UI step and output ONLY valid JSON matching this schema:

{
  "step_id": "string",
  "intent": "string",
  "action": "click|type|select|scroll|hover",
  "ui_element": {
    "type": "string",
    "label": "string",
    "visual_location": "string",
    "identification_strategy": ["string"]
  },
  "precondition": "string",
  "resulting_state": "string"
}

Input:
---
[paste step]
---
```

### Request Markdown Output

```
Analyze this flow and output in the canonical UI_FLOW markdown format.
Include all metadata, steps, and automation notes.

Input:
---
[paste flow]
---
```

### Request Code Output

```
Generate a complete Playwright test file for this UI flow.
Include:
- Proper test structure
- All step implementations
- Wait conditions
- Assertions for each resulting state

Flow:
---
[paste flow]
---
```

---

## 💡 Tips for Effective Prompts

1. **Be specific about output format** - JSON, Markdown, or code
2. **Include context** - App name, page location, user state
3. **Describe annotations clearly** - What each visual marker means
4. **Specify automation framework** - Playwright, Cypress, Selenium
5. **Mention edge cases** - Loading states, errors, variations
