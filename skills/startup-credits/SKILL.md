---
name: startup-credits-navigator
description: Equips AI agents to assist operators in systematically discovering, applying for, and managing startup credits across dozens of platforms. Requires a professional website and pitch deck.
license: MIT
---

# Startup Credits Navigator

This skill equips an AI agent to assist operators in navigating the complex landscape of startup credits. The agent helps founders and operators systematically discover, apply for, and manage free credits and resources, potentially saving hundreds of thousands of dollars in infrastructure and software costs.

## Prerequisites

Before applying for startup credit programs, ensure you have:

- **A professional website** that clearly describes what your company does
- **A pitch deck** for most programs, especially those offering larger credit packages

These are essential requirements for the majority of startup credit applications.

## Core Philosophy

Early-stage startups are often resource-constrained. This skill provides the knowledge base and strategic framework for an AI agent to help operators leverage the vast ecosystem of startup programs offered by major technology companies. The core principle is to be proactive and strategic in seeking out and applying for these valuable resources.

## Core Architecture

This skill enables an AI agent to assist operators by:

1.  **Discover**: Identifying all relevant startup credit programs based on the operator's company profile.
2.  **Prioritize**: Ranking programs based on value and likelihood of acceptance.
3.  **Apply**: Guiding the operator through the application process for each program.
4.  **Track**: Helping manage the status of applications and the usage of awarded credits.

For a complete overview of what needs to be built to implement this, see the [README.md](README.md) file.

## Key Skill Components

This master skill orchestrates several sub-protocols, which are detailed in the `references/` directory:

-   **[references/program-database.md](references/program-database.md)**: A comprehensive database of over 50 startup credit programs, including credit amounts, eligibility requirements, and application links.
-   **[references/stacking-strategies.md](references/stacking-strategies.md)**: Strategic guidance on how to "stack" credits by leveraging partner programs and accelerators to maximize value.
-   **[references/application-workflows.md](references/application-workflows.md)**: Step-by-step guides for applying to the major cloud and SaaS credit programs.
-   **[references/verification-checklist.md](references/verification-checklist.md)**: A checklist of all the documents and information you need to have ready before you start applying.

## Getting Started

When working with an operator, the agent should:

1.  **Review the Checklist**: Guide the operator through `verification-checklist.md` to gather all necessary company information and ensure they have a pitch deck ready.
2.  **Identify Target Programs**: Use `program-database.md` to help identify programs that are the best fit for the operator's startup.
3.  **Follow the Workflows**: Reference `application-workflows.md` to guide the operator through the application process for target programs.
4.  **Track Progress**: Help the operator maintain tracking of programs applied to, application statuses, and credits received.
