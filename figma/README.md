# Figma Skills

Skills for converting Figma designs into production-ready code with pixel-perfect accuracy using the Figma Model Context Protocol (MCP).

## Available Skills

### figma-to-code

Systematically converts Figma designs into code using an iterative comparison workflow. Automatically extracts design context, generates framework-specific code, and validates implementation against the source design through continuous comparison.

**Key capabilities:**
- Pixel-perfect design-to-code conversion
- Iterative validation loop ensures 1:1 accuracy
- Supports React, Vue, Svelte, Next.js, and more
- Extracts design tokens and variables
- Handles Code Connect mappings
- Works with components, pages, and design systems

**Use cases:**
- Converting Figma mockups to production code
- Building component libraries from Figma
- Maintaining design-code parity
- Extracting design system tokens
- Creating responsive layouts from designs

**What you get:**
- Complete workflow for MCP-based design-to-code
- Documentation of all 10+ Figma MCP tools
- Framework-specific examples (React, Vue, Svelte, Flutter, etc.)
- URL parsing guide for all Figma URL formats
- Validation strategies and quality guidelines

**Inspired by:** The need for pixel-perfect implementations that actually match designs, eliminating the back-and-forth between designers and developers.

---

## Philosophy

Design-to-code shouldn't be "close enough." With the Figma MCP, we can achieve exact pixel-perfect implementations by:

1. **Iterative Comparison**: Continuously fetching fresh design context and comparing against implementation
2. **Tool Mastery**: Leveraging all 10+ MCP tools strategically, not just one
3. **Framework Awareness**: Generating idiomatic code for the target framework
4. **Token Extraction**: Using actual design tokens instead of hardcoding values
5. **Validation First**: Comparing after each component, not at the end

The result is code that matches designs exactly, maintainable component architecture, and a systematic process that works every time.

---

## When to Use These Skills

Use Figma skills when:
- You have Figma design URLs and need to implement them in code
- Building UI components that must match designs exactly
- Setting up design systems from Figma libraries
- Converting mockups into responsive layouts
- Extracting design tokens for theming
- Establishing Code Connect mappings

Don't use when:
- You don't have access to the Figma file
- Designs are rough sketches without proper Figma components
- You're prototyping without design constraints
- Working with non-Figma design tools

---

## Key Differentiators

**vs. Manual Design Implementation:**
- Automated extraction of design context
- Exact values (no guessing spacing/colors)
- Faster iteration through MCP tools
- Built-in validation loop

**vs. Design-to-Code Tools:**
- More control over generated code quality
- Supports all major frameworks
- Iterative comparison for accuracy
- Produces production-ready, maintainable code

**vs. "Inspect" Mode:**
- Programmatic access to design data
- Can process entire pages/systems at once
- Supports automation and workflows
- Better for complex components and variants

---

## The Figma MCP Advantage

### What is Figma MCP?

The Figma Model Context Protocol (MCP) is an interface that allows AI agents to programmatically access Figma design files, extract context, and generate code. It provides:

- **Design Context**: Complete styling, layout, and component information
- **Screenshots**: Visual representations of any node
- **Metadata**: Structural hierarchy and node relationships
- **Variables**: Design tokens and theme values
- **Code Connect**: Mappings between Figma and code components
- **FigJam Support**: Access to whiteboarding and diagram content

### Why Use MCP vs. Manual Implementation?

| Manual | With Figma MCP |
|--------|----------------|
| Copy colors by eye | Extract exact hex values |
| Measure spacing manually | Get precise pixel values |
| Guess font weights | Know exact typography |
| Screenshot and compare | Automated comparison |
| One-time extraction | Continuous validation |
| Framework-agnostic HTML | Framework-specific code |

---

## Workflow Overview

### Standard Figma-to-Code Process

```
1. Receive Figma URL
   ↓
2. Parse URL → Extract fileKey & nodeId
   ↓
3. Get Metadata → Understand structure
   ↓
4. Get Design Context → Extract styling/layout
   ↓
5. Generate Code → Build component
   ↓
6. Get Design Context Again → Compare
   ↓
7. Fix Mismatches → Iterate
   ↓
8. Validate → Screenshot comparison
   ↓
9. Move to Next Component
```

### Design System Setup Process

```
1. Get Variable Definitions → Extract tokens
   ↓
2. Create Theme Configuration → CSS vars / config
   ↓
3. Get Code Connect Map → Check existing components
   ↓
4. For each component:
   ├─ Get Design Context
   ├─ Build Component
   ├─ Validate
   └─ Add Code Connect Mapping
   ↓
5. Create Design System Rules → Documentation
```

---

## Available MCP Tools

The Figma MCP provides 10+ specialized tools:

### Core Tools
1. **get_design_context** - Primary tool for design-to-code (generates code)
2. **get_metadata** - Structural overview in XML format
3. **get_screenshot** - Visual screenshots for comparison

### Design System Tools
4. **get_variable_defs** - Extract design tokens and variables
5. **create_design_system_rules** - Generate design system docs

### Code Connect Tools
6. **get_code_connect_map** - View Figma-to-code mappings
7. **add_code_connect_map** - Create new mappings

### FigJam Tools
8. **get_figjam** - Extract FigJam content
9. **generate_diagram** - Create Mermaid.js diagrams

### Utility Tools
10. **whoami** - Check authentication status

Each tool has specific use cases and best practices documented in the skill references.

---

## Supported Frameworks

The `figma-to-code` skill supports all major frameworks:

### Frontend Frameworks
- ✅ **React** (JavaScript & TypeScript)
- ✅ **Next.js** (App Router & Pages Router)
- ✅ **Vue 3** (Composition & Options API)
- ✅ **Svelte** (& SvelteKit)
- ✅ **HTML + CSS** (Vanilla)
- ✅ **Web Components**

### Mobile Frameworks
- ✅ **React Native**
- ✅ **Flutter** (Dart)
- ✅ **Swift UIKit**
- ✅ **SwiftUI**
- ✅ **Kotlin Compose**

### CSS Solutions
- ✅ **Tailwind CSS**
- ✅ **CSS Modules**
- ✅ **Styled Components**
- ✅ **Emotion**
- ✅ **Vanilla CSS**

See [framework-examples.md](./figma-to-code/references/framework-examples.md) for detailed examples.

---

## Success Metrics

You're successfully using Figma skills when:

- ✅ **Pixel-perfect**: Implementation matches design exactly (not "close enough")
- ✅ **Iterative**: You call MCP tools multiple times during implementation
- ✅ **Validated**: Each component is compared against design before moving on
- ✅ **Token-based**: Using design variables, not hardcoded values
- ✅ **Production-ready**: Code is clean, typed, and maintainable
- ✅ **Framework-idiomatic**: Code follows best practices for the target framework

---

## Common Use Cases

### Use Case 1: Landing Page Implementation
```
1. Receive Figma URL for landing page
2. Use get_metadata to see Hero, Features, CTA, Footer sections
3. For each section:
   - Get design context
   - Build component
   - Validate with fresh design context
   - Take screenshot for visual comparison
4. Assemble full page
5. Final validation
```

### Use Case 2: Design System Setup
```
1. Get variable definitions for all tokens
2. Create theme configuration file
3. Check Code Connect for existing mappings
4. Build core components (Button, Input, Card, etc.)
5. Add Code Connect mappings for each
6. Generate design system documentation
```

### Use Case 3: Complex Component with Variants
```
1. Get metadata to identify variant nodes
2. Get design context for base component
3. Get design context for each variant
4. Build component with variant logic
5. Validate each variant individually
6. Test all combinations
```

### Use Case 4: Responsive Design
```
1. Get design context for desktop (node A)
2. Get design context for tablet (node B)
3. Get design context for mobile (node C)
4. Build responsive component with breakpoints
5. Validate at each breakpoint
```

---

## Quality Standards

### Generated Code Must:

1. **Match design exactly** - Pixel-perfect, not approximate
2. **Be production-ready** - No TODOs or placeholder comments
3. **Be typed** - Use TypeScript when appropriate
4. **Be accessible** - WCAG compliant with proper ARIA
5. **Be semantic** - Proper HTML elements and structure
6. **Be maintainable** - Clear naming, organized structure
7. **Be performant** - Optimized images, efficient styles
8. **Follow conventions** - Match project's coding standards

### Validation Checklist:

- ✅ Spacing matches (margins, padding, gaps)
- ✅ Colors match (text, backgrounds, borders)
- ✅ Typography matches (sizes, weights, line-heights)
- ✅ Layout matches (flexbox, grid, positioning)
- ✅ Dimensions match (widths, heights)
- ✅ Border radius matches
- ✅ Shadows match
- ✅ Responsive behavior works

---

## Future Skills

Potential additions to this category:

- **figma-components-audit** - Analyze Figma files for component usage and consistency
- **figma-design-lint** - Validate design files against design system rules
- **figma-to-storybook** - Generate Storybook stories from Figma components
- **figma-version-diff** - Compare design changes between Figma versions
- **figma-accessibility-check** - Audit designs for accessibility issues

---

## Resources

### Official Documentation
- [Figma MCP Server Docs](https://developers.figma.com/docs/figma-mcp-server)
- [Figma Plugin API](https://www.figma.com/plugin-docs/)
- [Figma Design Tokens](https://help.figma.com/hc/en-us/articles/15339657135383-Guide-to-variables-in-Figma)

### Community Resources
- [Figma Community](https://www.figma.com/community)
- [Design Systems Repo](https://designsystemsrepo.com/)
- [Awesome Design Systems](https://github.com/alexpate/awesome-design-systems)

### Related Tools
- [Figma Tokens Plugin](https://www.figma.com/community/plugin/843461159747178978/Figma-Tokens)
- [Code Connect](https://www.figma.com/developers/code-connect)
- [Storybook Figma Plugin](https://storybook.js.org/addons/storybook-addon-designs)

---

## Getting Started

Ready to convert Figma designs to code?

1. **Get a Figma URL** - Design file, component, or page
2. **Load the skill** - Use `figma-to-code` skill in Claude or Cursor
3. **Provide the URL** - Share the Figma link
4. **Specify framework** - Tell the agent what you're building (React, Vue, etc.)
5. **Watch it work** - The agent will extract, generate, and validate iteratively

The agent will automatically:
- Parse the URL to extract fileKey and nodeId
- Fetch design context from Figma
- Generate framework-specific code
- Validate against the design
- Iterate until pixel-perfect

---

**Pro tip:** The key to success with Figma MCP is the iterative comparison loop. Always fetch design context multiple times during implementation, not just once at the start.

---

Made with ❤️ by the team at [hyperfx.ai](https://hyperfx.ai)

