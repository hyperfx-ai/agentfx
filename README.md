# AgentFX

**The AI Agent Component Library**

[![Website](https://img.shields.io/badge/website-agentfx.directory-blue)](https://agentfx.directory)
[![License](https://img.shields.io/badge/license-Apache%202.0-green)](LICENSE)
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[![Follow on X](https://img.shields.io/twitter/follow/hyperfxai?style=social)](https://twitter.com/hyperfxai)

---

A curated collection of verified skills and resources for AI development. Works in Cursor, Claude, Hyper, and anywhere else you build with AI.

## Contents

- [What Are Claude Skills?](#what-are-claude-skills)
- [Skills](#skills)
  - [Polymarket](#polymarket)
  - [Startups](#startups)
  - [B2B Growth](#b2b-growth)
  - [Content](#content)
  - [Figma](#figma)
  - [Next.js](#nextjs)
- [Getting Started](#getting-started)
- [Creating Skills](#creating-skills)
- [Contributing](#contributing)
- [Resources](#resources)
- [Installation](#installation)
- [License](#license)

## What Are Claude Skills?

Claude Skills are customizable workflows that teach Claude how to perform specific tasks according to your unique requirements. Skills enable Claude to execute tasks in a repeatable, standardized manner across all Claude platforms.

**AgentFX** is our attempt to filter the noise. We ship verified work—ours and others'—with proper attribution. The curation is the value.

This repository contains production-ready skills (markdown files with instructions for AI agents). Each skill has been tested and is actively used in production environments. Use them anywhere. More components coming soon.

These skills are derived from agents and workflows already running on **[HyperFX.ai](https://hyperfx.ai)**. Sign up for a free trial and DM us on [Twitter](https://twitter.com/hyperfxai) for free credits.

Read more: [AgentFX Day Zero](https://hyperfx.ai/blog/agentfx-day-zero)

## Skills

### Polymarket

Algorithmic trading skills for prediction markets. Built on an event-driven architecture for speed and efficiency. [Learn more →](./polymarket)

* [news-monitoring](./polymarket/news-monitoring) - Monitors real-time information sources and identifies market-moving news.
* [scalp-trading](./polymarket/scalp-trading) - Executes short-term trades based on new information before markets reprice.
* [arbitrage-scanner](./polymarket/arbitrage-scanner) - Continuously scans for risk-free arbitrage opportunities.
* [position-management](./polymarket/position-management) - Tracks positions and executes exits with mechanical precision.

### Startups

Skills for early-stage startup operations, focusing on maximizing resources and extending runway. [Learn more →](./startups)

* [credit-discovery](./startups/credit-discovery) - Discovers and prioritizes startup credit programs across 50+ platforms.
* [credit-application](./startups/credit-application) - Guides through application process with step-by-step workflows.

### B2B Growth

Proven B2B SaaS growth tactics with copy-paste templates across paid, outbound, and organic channels. [Learn more →](./b2b-growth)

* [paid-ads](./b2b-growth/paid-ads) - Templates for Facebook, Google Search, and LinkedIn ads.
* [cold-outbound](./b2b-growth/cold-outbound) - Cold email templates and LinkedIn outreach sequences.
* [organic-content](./b2b-growth/organic-content) - Templates for podcasts, newsletters, and YouTube.

### Content

Skills for creating high-quality content that prioritizes clarity and simplicity. [Learn more →](./content)

* [clear-content](./content/clear-content) - Generate clear, direct content for blogs, social media, and marketing copy.

### Figma

Skills for converting Figma designs into pixel-perfect code using the Figma Model Context Protocol. [Learn more →](./figma)

* [figma-to-code](./figma/figma-to-code) - Systematically convert Figma designs to production-ready code with iterative validation.

### Next.js

Skills for building, optimizing, and maintaining Next.js applications. [Learn more →](./nextjs)

* [nextjs-seo](./nextjs/nextjs-seo) - Optimize Next.js apps with JSON-LD, metadata API, and schema.org structured data.

## Getting Started

### Using Skills in Claude.ai

1. Click the skill icon (🧩) in your chat interface.
2. Add skills from the marketplace or upload custom skills.
3. Claude automatically activates relevant skills based on your task.

### Using Skills in Claude Code

1. Place the skill in `~/.config/claude-code/skills/`:

```bash
mkdir -p ~/.config/claude-code/skills/
cp -r category-name/skill-name ~/.config/claude-code/skills/
```

2. Verify skill metadata:

```bash
head ~/.config/claude-code/skills/skill-name/SKILL.md
```

3. Start Claude Code:

```bash
claude
```

4. The skill loads automatically and activates when relevant.

### Using Skills via API

Use the Claude Skills API to programmatically load and manage skills:

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

response = client.messages.create(
    model="claude-3-5-sonnet-20241022",
    skills=["skill-id-here"],
    messages=[{"role": "user", "content": "Your prompt"}]
)
```

See the [Skills API documentation](https://docs.anthropic.com/claude/docs/skills) for details.

## Creating Skills

### Skill Structure

Each skill is a folder containing a `SKILL.md` file with YAML frontmatter:

```
category-name/
└── skill-name/
    ├── SKILL.md          # Required: Skill instructions and metadata
    ├── references/       # Optional: Reference files and protocols
    └── LICENSE.txt       # Optional: Custom license
```

### Basic Skill Template

```markdown
---
name: my-skill-name
description: A clear description of what this skill does and when to use it.
category: "Business & Marketing"
keywords: ["keyword1", "keyword2", "keyword3"]
license: MIT
---

# My Skill Name

Detailed description of the skill's purpose and capabilities.

## When to Use This Skill

- Use case 1
- Use case 2
- Use case 3

## How to Use This Skill

[Detailed instructions for Claude on how to execute this skill]

## Example Usage

[Real-world example showing input and output]

## Reference Resources

[Link to any reference files in the references/ directory]
```

### Skill Best Practices

* Focus on specific, repeatable tasks
* Include clear examples and edge cases
* Write instructions for Claude, not end users
* Test across Claude.ai, Claude Code, and API
* Document prerequisites and dependencies
* Include error handling guidance

## Contributing

We welcome contributions! Here's how to contribute:

### Quick Contribution Steps

1. Ensure your skill is based on a real use case
2. Check for duplicates in existing skills
3. Follow the skill structure template
4. Test your skill across platforms
5. Submit a pull request with clear documentation

### Contribution Guidelines

* **Quality over quantity**: Only submit skills you've actually used in production
* **Proper attribution**: Credit original authors and sources
* **Clear documentation**: Include examples and edge cases
* **Test thoroughly**: Verify skills work across Claude.ai, Claude Code, and API
* **Follow the template**: Maintain consistent structure across all skills

## Resources

### Official Documentation

* [Claude Skills Overview](https://www.anthropic.com/news/claude-skills) - Official announcement and features
* [Skills User Guide](https://support.anthropic.com/claude/skills) - How to use skills in Claude
* [Creating Custom Skills](https://docs.anthropic.com/claude/docs/creating-skills) - Skill development guide
* [Skills API Documentation](https://docs.anthropic.com/claude/docs/skills) - API integration guide

### Community Resources

* [Anthropic Skills Repository](https://github.com/anthropics/skills) - Official example skills
* [Awesome Claude Skills](https://github.com/ComposioHQ/awesome-claude-skills) - Community curated skills
* [AgentFX Directory](https://agentfx.directory) - Browse and discover skills

### Inspiration & Use Cases

* [Lenny's Newsletter](https://www.lennysnewsletter.com/p/claude-code) - 50 ways people use Claude Code
* [AgentFX Day Zero](https://hyperfx.ai/blog/agentfx-day-zero) - How we're building AgentFX

## Installation

Clone and use anywhere:

```bash
git clone https://github.com/hyperfx/agentfx.git
```

CLI tool coming soon for npm/npx installation.

## Join the Community

* Questions or feedback? Reach out on [Twitter](https://twitter.com/hyperfxai)
* Want to contribute? Submit a pull request or open an issue
* Looking for more? Visit [agentfx.directory](https://agentfx.directory)

## License

This repository is licensed under the Apache License 2.0.

Individual skills may have different licenses - please check each skill's folder for specific licensing information.

---

**Note**: Claude Skills work across Claude.ai, Claude Code, and the Claude API. Once you create a skill, it's portable across all platforms, making your workflows consistent everywhere you use Claude.

Built with ❤️ by the team at [hyperfx.ai](https://hyperfx.ai)
