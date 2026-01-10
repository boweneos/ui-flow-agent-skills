# Multimodal UI Flow Analyzer - Agent Skill

An Agent Skill for analyzing annotated UI screenshots and markdown documentation to generate agent-consumable UI flow specifications.

## 📁 Skill Structure

```
multimodal-ui-flow-analyzer/
├── SKILL.md                          # Main skill instructions
├── README.md                         # This file
├── assets/
│   └── templates/
│       ├── step-output.json          # JSON schema for step output
│       ├── flow-output.md            # Template for complete flow docs
│       └── automation-hints.md       # Template for test automation
└── references/
    └── original-guide.md             # Reference to source methodology
```

## 🚀 Quick Start

### Prerequisites

- An AI agent that supports the Agent Skills format (e.g., Cursor, Claude)
- UI documentation with annotated screenshots
- Markdown files describing UI flows

### Triggering the Skill

The skill activates when you ask the agent to:
- Analyze UI flows from screenshots
- Convert UI documentation to structured specs
- Generate automation-ready UI interaction sequences

---

## 📝 Trigger Prompts

Use these prompts to activate and use the skill effectively:

### Basic Analysis

```
Analyze this UI flow documentation and generate a structured specification:
[paste your markdown with screenshot references]
```

### Full Flow Conversion

```
Using the multimodal-ui-flow-analyzer skill, convert the following UI walkthrough 
into an agent-consumable flow specification. Include automation hints for each step.

[paste your UI documentation]
```

### Single Step Analysis

```
Analyze this single UI step and output the structured JSON format:

## Step 3: Submit Form

**Intent:** Submit the completed registration form

**User Action:** Click the "Register" button at the bottom of the form

**Visual Reference:** [screenshot with red box around Register button]

**Visual Annotations:**
- Red box highlights the Register button
- Arrow shows click target
```

### Generate Automation Hints

```
For the following UI flow, generate detailed automation hints including 
Playwright selectors and wait strategies:

[paste your flow specification]
```

### Batch Processing

```
I have multiple UI flows documented in markdown files. For each file in the 
new_business/ folder, analyze the UI steps and generate structured specifications.
```

---

## 📋 Input Format Requirements

For best results, structure your UI documentation as follows:

```markdown
## Step N: <Short Title>

**Intent:**
What the user is trying to accomplish.

**User Action (Text):**
Plain-language description of the interaction.

**Visual Reference:**
![step-n](path/to/screenshot.png)

**Visual Annotations:**
- Red box around target element
- Arrow indicating click direction
- Highlight on relevant area
```

---

## 📤 Output Formats

### 1. Step JSON (for programmatic use)

```json
{
  "step_id": "step-1",
  "intent": "Navigate to dashboard",
  "action": "click",
  "ui_element": {
    "type": "link",
    "label": "Dashboard",
    "visual_location": "left sidebar",
    "identification_strategy": [
      "role=link with name 'Dashboard'",
      "data-testid='nav-dashboard'"
    ]
  },
  "precondition": "User is logged in",
  "resulting_state": "Dashboard page is displayed"
}
```

### 2. Flow Markdown (for documentation)

See `assets/templates/flow-output.md` for the complete template.

### 3. Automation Hints (for test engineers)

See `assets/templates/automation-hints.md` for selector strategies and code examples.

---

## 🎯 Example Use Cases

### Use Case 1: Document a New Feature

```
I've added a new "Export to PDF" feature. Here's the UI flow with screenshots.
Please analyze and generate:
1. Structured flow specification
2. Playwright test skeleton
3. Accessibility considerations
```

### Use Case 2: Audit Existing Flows

```
Review the UI flows in our documentation folder. For each flow:
- Validate step ordering
- Check for missing preconditions
- Flag ambiguous UI references
- Suggest selector improvements
```

### Use Case 3: Cross-Platform Consistency

```
Analyze these UI flows from our web app. Generate specifications that could be 
used to verify the same flows work correctly on our mobile app.
```

---

## ⚙️ Integration with Agent Systems

### Cursor IDE

Install via GitHub URL in Cursor Settings → Rules → Add Rule → Remote Rule (Github).
Reference it directly:

```
@multimodal-ui-flow-analyzer analyze the login flow in docs/login-flow.md
```

### Claude (via XML injection)

Add to your system prompt:

```xml
<available_skills>
  <skill>
    <name>multimodal-ui-flow-analyzer</name>
    <description>Analyze annotated UI screenshots and markdown to generate 
    agent-consumable UI flow specifications.</description>
    <location>/path/to/multimodal-ui-flow-analyzer/SKILL.md</location>
  </skill>
</available_skills>
```

### Custom Agents

Load the skill programmatically:

```python
from pathlib import Path

skill_path = Path("multimodal-ui-flow-analyzer/SKILL.md")
skill_content = skill_path.read_text()

# Inject into agent context when UI analysis is needed
agent.add_context(skill_content)
```

---

## 🔧 Customization

### Adding Custom Templates

Create new templates in `assets/templates/` for your specific needs:
- Test framework-specific output
- Documentation format variations
- CI/CD integration specs

### Extending the Skill

Modify `SKILL.md` to add:
- Additional output formats
- Domain-specific annotation rules
- Custom validation checks

---

## 📚 Related Resources

- [Agent Skills Specification](https://agentskills.io/specification)
- [Original Methodology Guide](../multimodal_ui_flow_llm_guide.md)
- [Playwright Selectors Guide](https://playwright.dev/docs/selectors)

---

## 🤝 Contributing

To improve this skill:

1. Test with diverse UI documentation
2. Add edge case handling to `SKILL.md`
3. Create new output templates
4. Document additional trigger prompts

---

## 📄 License

MIT License - See skill metadata in `SKILL.md`
