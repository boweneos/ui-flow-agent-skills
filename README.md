# Multimodal UI Flow Analyzer - Agent Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-blue)](https://agentskills.io)

> Analyze annotated UI screenshots and markdown documentation to generate agent-consumable UI flow specifications for AI agents.

## 🎯 Overview

This is a production-ready [Agent Skill](https://agentskills.io) that extends AI agent capabilities with specialized knowledge for UI flow analysis and automation. Built on the open Agent Skills format, this skill works across multiple agent platforms including Cursor, Claude, and custom AI systems.

### What are Agent Skills?

Agent Skills are a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows. They're portable, version-controlled packages that give agents access to procedural knowledge and context they need to perform complex tasks reliably.

## ✨ Capabilities

Analyze annotated UI screenshots and markdown documentation to generate agent-consumable UI flow specifications.

**Use Cases:**
- 🎨 Convert visual UI documentation into structured automation specs
- 🤖 Generate test automation guidance from UI walkthroughs  
- 📋 Create machine-readable UI interaction sequences
- ✅ Validate UI flow completeness and consistency

**Key Features:**
- Vision-aware analysis of annotated screenshots
- Structured JSON and Markdown output formats
- Automation hints for Playwright, Cypress, and other frameworks
- Semantic selector recommendations
- Precondition and postcondition tracking

[**📖 View Detailed Skill Documentation →**](./skill-README.md)

## 🚀 Quick Start

### Prerequisites

- An AI agent that supports the Agent Skills format (e.g., [Cursor](https://cursor.sh), Claude)
- For the UI Flow Analyzer: Vision-capable AI model (Claude 3.5 Sonnet, GPT-4 Vision, etc.)

### Installation

#### Option 1: Clone the Repository

```bash
git clone https://github.com/boweneos/ui-flow-agent-skills.git
cd ui-flow-agent-skills
```

#### Option 2: Add as Git Submodule

```bash
cd your-project
git submodule add https://github.com/boweneos/ui-flow-agent-skills.git skills
```

#### Option 3: Direct Download

Download specific skills directly from the [releases page](https://github.com/boweneos/ui-flow-agent-skills/releases).

### Usage

#### In Cursor IDE

1. Open **Cursor Settings** (Cmd+Shift+J)
2. Navigate to **Rules** → **Add Rule** → **Remote Rule (Github)**
3. Enter: `https://github.com/boweneos/ui-flow-agent-skills`
4. The skill will be available as `@multimodal-ui-flow-analyzer`

```
@multimodal-ui-flow-analyzer analyze the login flow in docs/login-flow.md
```

#### With Claude (via XML injection)

Add to your system prompt:

```xml
<available_skills>
  <skill>
    <name>multimodal-ui-flow-analyzer</name>
    <description>Analyze annotated UI screenshots and markdown to generate agent-consumable UI flow specifications.</description>
    <location>/path/to/ui-flow-agent-skills/SKILL.md</location>
  </skill>
</available_skills>
```

#### With Custom Agents

Load skills programmatically:

```python
from pathlib import Path

skill_path = Path("ui-flow-agent-skills/SKILL.md")
skill_content = skill_path.read_text()

# Inject into agent context when needed
agent.add_context(skill_content)
```

## 📚 Documentation

- **[Agent Skills Specification](https://agentskills.io/specification)** - Official format specification
- **[Integration Guide](https://agentskills.io/integrate-skills)** - How to add skills to your agent
- **[Example Skills](https://github.com/anthropics/agent-skills)** - More skill examples

## 🏗️ Repository Structure

```
ui-flow-agent-skills/
├── SKILL.md                         # Skill instructions + metadata (REQUIRED at root)
├── README.md                        # This file
├── skill-README.md                  # Detailed skill documentation
├── LICENSE                          # MIT License
├── assets/                          # Templates and resources
│   ├── PROMPTS.md
│   └── templates/
│       ├── step-output.json
│       ├── flow-output.md
│       └── automation-hints.md
└── references/                      # Reference materials
    └── original-guide.md
```

**Note:** SKILL.md must be at the repository root for Cursor's GitHub import to discover it correctly.

## 🛠️ Creating Your Own Skills

Want to create custom skills? Follow the [Agent Skills specification](https://agentskills.io/specification):

1. Create a directory with your skill name (lowercase, hyphens only)
2. Add a `SKILL.md` file with YAML frontmatter:

```yaml
---
name: your-skill-name
description: What your skill does and when to use it
license: MIT
metadata:
  author: your-name
  version: "1.0"
---

# Your Skill Instructions

Write detailed instructions for the AI agent here...
```

3. Optionally add supporting files:
   - `scripts/` - Executable code
   - `references/` - Documentation
   - `assets/` - Templates and resources

4. Validate your skill:

```bash
pip install skills-ref
skills-ref validate your-skill-name
```

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Report Issues** - Found a bug or have a suggestion? [Open an issue](https://github.com/boweneos/ui-flow-agent-skills/issues)
2. **Improve Existing Skills** - Submit PRs to enhance current skills
3. **Add New Skills** - Create new skills following the specification
4. **Share Feedback** - Let us know how you're using these skills

### Development Workflow

```bash
# Fork and clone the repository
git clone https://github.com/YOUR_USERNAME/ui-flow-agent-skills.git
cd ui-flow-agent-skills

# Create a feature branch
git checkout -b feature/your-feature-name

# Make your changes and test them

# Validate your changes
skills-ref validate your-skill-name

# Commit and push
git add .
git commit -m "Description of your changes"
git push origin feature/your-feature-name

# Open a Pull Request
```

## 📋 Validation

All skills in this repository are validated against the Agent Skills specification:

```bash
# Install validation tool
pip install skills-ref

# Validate all skills
skills-ref validate .

# Validate specific skill
skills-ref validate multimodal-ui-flow-analyzer
```

## 🔗 Related Projects

- [Agent Skills Specification](https://github.com/anthropics/agent-skills) - Official specification and examples
- [skills-ref](https://github.com/anthropics/skills-ref) - Python reference implementation
- [Cursor](https://cursor.sh) - AI-first code editor with skills support
- [Anthropic Claude](https://www.anthropic.com/claude) - AI assistant with skills support

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built on the [Agent Skills](https://agentskills.io) open standard by Anthropic
- Inspired by the need for portable, reusable AI agent capabilities
- Thanks to the open-source community for feedback and contributions

## 📞 Support

- **Issues:** [GitHub Issues](https://github.com/boweneos/ui-flow-agent-skills/issues)
- **Discussions:** [GitHub Discussions](https://github.com/boweneos/ui-flow-agent-skills/discussions)
- **Documentation:** [Agent Skills Documentation](https://agentskills.io)

---

<div align="center">

**[⭐ Star this repo](https://github.com/boweneos/ui-flow-agent-skills)** if you find it useful!

Made with ❤️ for the AI agent community

</div>
