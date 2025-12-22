# AgentFX

**The AI Agent Component Library**

A curated collection of agents, skills, and resources for AI development. Works in Cursor, Claude, Hyper, and anywhere else you build.

## What This Is

AgentFX is our attempt to filter the noise. We ship verified work—ours and others'—with proper attribution. The curation is the value.

This repository contains production-ready skills (markdown files with instructions for AI agents). Use them anywhere. More components coming soon.

Read more: [AgentFX Day Zero](https://hyperfx.ai/blog/agentfx-day-zero)

## Try in Hyper

These skills are derived from agents and workflows already running on **[HyperFX.ai](https://hyperfx.ai)**. Sign up for a free trial and DM us on [Twitter](https://twitter.com/hyperfxai) for free credits.

## Installation

Clone and use anywhere:

```bash
git clone https://github.com/hyper/agentfx.git
```

Or install in Claude Code:

```
/plugin marketplace add hyper/agentfx
/plugin install trading-skills@agentfx-skills
/plugin install startup-credits@agentfx-skills
/plugin install b2b-growth@agentfx-skills
```

CLI tool coming soon for npm/npx installation.

## Creating Skills

Skills are just markdown files with YAML frontmatter. See [./template](./template) for a starting point.

## Available Skills

**Trading**
- [polymarket](./skills/polymarket) - High-frequency scalp trading on Polymarket

**Startup & Business**
- [startup-credits](./skills/startup-credits) - Discover and apply for startup credits (requires website + deck)
- [b2b-growth](./skills/b2b-growth) - Proven B2B SaaS growth tactics across paid ads, organic, and outbound

*More coming soon*

## License

Apache 2.0
