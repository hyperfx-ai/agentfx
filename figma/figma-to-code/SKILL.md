---
name: figma-to-code
description: This skill enables pixel-perfect design-to-code conversion from Figma using the Figma MCP server. Use this skill when provided with Figma design links to systematically extract design context, generate code, and iterate with continuous comparison to ensure 1:1 accuracy between design and implementation.
category: "Development"
keywords: ["figma", "design-to-code", "MCP", "UI development", "component generation", "design systems", "frontend"]
license: MIT
---

# Figma to Code with MCP

Systematically converts Figma designs into pixel-perfect code using the Figma Model Context Protocol (MCP) with an iterative comparison workflow.

## When to Use This Skill

This skill should be used when:
- Given a Figma design URL to implement in code
- Converting Figma components, frames, or pages into frontend code
- Building UI that must match Figma designs exactly
- Extracting design tokens, variables, and styles from Figma
- Maintaining 1:1 parity between Figma designs and code implementation

## Core Principle

**Iterative Comparison Loop**: After generating each component or section, immediately fetch the latest design context from Figma to compare against what was built. This continuous validation ensures pixel-perfect accuracy and catches mismatches early.

## Understanding Figma URLs

Figma URLs follow this pattern:
```
https://figma.com/design/:fileKey/:fileName?node-id=1-2
```

**Extract these values:**
- `fileKey`: The unique file identifier (e.g., `pqrs`)
- `nodeId`: The node identifier from the URL (e.g., `1-2` becomes `1:2`)

**Branch URLs:**
```
https://figma.com/design/:fileKey/branch/:branchKey/:fileName
```
When a branch is present, use `branchKey` as the `fileKey`.

## Available MCP Tools

### Primary Tools (Use These First)

#### 1. `get_design_context`
**Purpose**: The main tool for getting design information and generating code.

**Returns**:
- Generated code (HTML/CSS/React/etc.) based on the Figma node
- Asset download URLs for images, icons, etc.
- Component structure and hierarchy
- Code Connect mappings (if available)

**Parameters**:
- `fileKey` (required): Figma file key
- `nodeId` (required): Node ID in format `123:456`
- `clientLanguages`: Programming languages (e.g., `javascript`, `typescript`)
- `clientFrameworks`: Frameworks (e.g., `react`, `vue`, `svelte`)
- `disableCodeConnect`: Set to true to disable Code Connect
- `forceCode`: Force code generation even for large outputs

**When to use**: This is your primary tool. Use it first when starting implementation and after each section is built to refresh context.

#### 2. `get_metadata`
**Purpose**: Get structural overview of a Figma page or node in XML format.

**Returns**:
- Node IDs, layer types, names
- Positions and sizes
- Component hierarchy
- Does NOT include detailed styling

**Parameters**:
- `fileKey` (required)
- `nodeId` (required)

**When to use**: 
- To understand the overall structure before diving into implementation
- To get node IDs of child elements
- When you need to navigate a complex design
- For large files where you need an overview first

#### 3. `get_screenshot`
**Purpose**: Generate visual screenshots of Figma nodes.

**Parameters**:
- `fileKey` (required)
- `nodeId` (required)

**When to use**:
- To visually inspect what a node looks like
- To compare your implementation against the design
- To show the user what you're looking at
- During debugging when code doesn't match design

### Supporting Tools

#### 4. `get_variable_defs`
**Purpose**: Extract design tokens and variables from Figma.

**Returns**: Variable definitions like `{'icon/default/secondary': #949494}`

**When to use**:
- Setting up design tokens in your codebase
- Creating CSS variables or theme files
- Ensuring color/spacing consistency

#### 5. `get_code_connect_map`
**Purpose**: Get mapping of Figma node IDs to existing code components.

**Returns**: `{'1:2': { codeConnectSrc: 'components/Button.tsx', codeConnectName: 'Button' }}`

**Parameters**:
- `fileKey` (required)
- `nodeId` (required)
- `codeConnectLabel`: Language/framework label when multiple mappings exist

**When to use**:
- To see if Figma components are already mapped to code
- To understand existing component architecture
- Before building new components (check if they already exist)

#### 6. `add_code_connect_map`
**Purpose**: Create mapping between Figma nodes and code components.

**Parameters**:
- `fileKey` (required)
- `nodeId` (required)
- `source`: Location of component in codebase
- `componentName`: Name of the component
- `label`: Framework label (React, Vue, Svelte, etc.)

**When to use**:
- After creating a new component from a Figma design
- To establish connections for future reference
- Building design system documentation

#### 7. `create_design_system_rules`
**Purpose**: Generate design system rules for the repository.

**When to use**:
- Setting up a new design system
- Documenting design patterns
- Creating style guides

### FigJam-Specific Tools

#### 8. `get_figjam`
**Purpose**: Extract content from FigJam files (different from Figma design files).

**Important**: Only works with FigJam URLs (`figma.com/board/...`)

#### 9. `generate_diagram`
**Purpose**: Create diagrams in FigJam using Mermaid.js syntax.

**Supports**: Flowcharts, sequence diagrams, state diagrams, gantt charts

**When to use**:
- Creating technical diagrams
- Documenting workflows
- Planning system architecture

### Utility Tools

#### 10. `whoami`
**Purpose**: Check authenticated user information.

**When to use**:
- Debugging permission issues
- Validating authentication

## The Iterative Implementation Workflow

### Step 1: Initial Analysis
```
1. Receive Figma URL from user
2. Extract fileKey and nodeId from the URL
3. Call get_metadata to understand overall structure
4. Identify main sections/components to build
5. Call get_screenshot to visually inspect the design
```

### Step 2: Get Design Context
```
6. Call get_design_context with:
   - Extracted fileKey and nodeId
   - User's preferred framework (React, Vue, etc.)
   - User's preferred language (TypeScript, JavaScript, etc.)
7. Review the generated code structure
8. Check for Code Connect mappings
9. Get variable definitions if design tokens are needed
```

### Step 3: Implement First Component/Section
```
10. Start implementing the first component based on design context
11. Focus on one component or section at a time
12. Write clean, production-ready code
```

### Step 4: Compare & Validate (CRITICAL)
```
13. Call get_design_context again with the same nodeId
14. Compare the fresh design data with what you just built
15. Check for discrepancies:
    - Spacing (margins, padding, gaps)
    - Colors (backgrounds, text, borders)
    - Typography (font sizes, weights, line heights)
    - Layout (flexbox, grid, positioning)
    - Dimensions (widths, heights)
16. Take a screenshot if visual comparison is needed
```

### Step 5: Iterate & Refine
```
17. Fix any mismatches found in Step 4
18. Call get_design_context again to revalidate
19. Repeat until component matches 1:1
```

### Step 6: Move to Next Component
```
20. Once current component is pixel-perfect, move to next section
21. Repeat Steps 3-5 for each component
22. Use get_metadata to navigate to child nodes if needed
```

### Step 7: Final Validation
```
23. Call get_design_context on the parent node (full page/section)
24. Ensure all components work together correctly
25. Take final screenshots for comparison
26. Verify responsive behavior if applicable
```

## Implementation Guidelines

### Always Do This

1. **Call MCP tools multiple times**: Don't rely on a single call at the start. Refresh context continuously.

2. **Be specific with frameworks**: Always specify `clientFrameworks` and `clientLanguages` in `get_design_context` calls for accurate code generation.

3. **Start with structure**: Use `get_metadata` first on complex designs to understand the hierarchy.

4. **Compare after each component**: Never build multiple components without validating each one individually.

5. **Use exact values**: Copy exact spacing, colors, and dimensions from Figma. Don't approximate.

6. **Handle assets properly**: Download and reference images/icons from the asset URLs provided.

7. **Respect design tokens**: Use `get_variable_defs` to extract variables before hardcoding values.

8. **Document mappings**: Use `add_code_connect_map` after creating components for future reference.

### Never Do This

1. **Don't guess**: If the design context is unclear, call `get_design_context` or `get_metadata` again.

2. **Don't skip validation**: Always compare your implementation against the design after building.

3. **Don't build everything at once**: Iterative, component-by-component approach is critical.

4. **Don't ignore Code Connect**: Check for existing mappings before recreating components.

5. **Don't approximate**: "Close enough" is not acceptable. Aim for pixel-perfect accuracy.

6. **Don't cache old data**: Always fetch fresh design context when comparing.

## Example Workflow

**User provides**: `https://figma.com/design/abc123/MyApp?node-id=10-25`

```markdown
1. Extract: fileKey = "abc123", nodeId = "10:25"

2. Get structure:
   get_metadata(fileKey="abc123", nodeId="10:25")
   → Shows page has 3 main sections: Header, Content, Footer

3. Get visual:
   get_screenshot(fileKey="abc123", nodeId="10:25")
   → Screenshot shows overall layout

4. Get design context for Header:
   get_metadata(fileKey="abc123", nodeId="10:25")
   → Find Header nodeId: "10:26"
   
   get_design_context(fileKey="abc123", nodeId="10:26", 
                      clientFrameworks="react", 
                      clientLanguages="typescript")
   → Returns React/TypeScript code for Header

5. Implement Header component

6. Validate Header:
   get_design_context(fileKey="abc123", nodeId="10:26")
   → Compare against implementation
   → Fix padding mismatch found
   
   get_design_context(fileKey="abc123", nodeId="10:26")
   → Revalidate - now matches perfectly

7. Move to Content section:
   Repeat steps 4-6 for nodeId "10:27"

8. Move to Footer section:
   Repeat steps 4-6 for nodeId "10:28"

9. Final validation:
   get_design_context(fileKey="abc123", nodeId="10:25")
   get_screenshot(fileKey="abc123", nodeId="10:25")
   → Confirm entire page matches design
```

## Handling Common Scenarios

### Large Pages with Many Components

```
1. Use get_metadata to get full structure
2. Break down into logical sections
3. Process each section independently
4. Validate section by section
5. Assemble at the end
```

### Complex Component with Variants

```
1. Get design context for base component
2. Use get_metadata to identify variant nodes
3. Get design context for each variant
4. Build component with variant logic
5. Validate each variant individually
```

### Design System Setup

```
1. Call get_variable_defs for design tokens
2. Set up CSS variables or theme configuration
3. Call get_code_connect_map to see existing mappings
4. Build base components
5. Use add_code_connect_map to link components
6. Use create_design_system_rules for documentation
```

### Responsive Designs

```
1. Get design context for desktop version
2. Get design context for mobile version (different nodeId)
3. Implement responsive logic (media queries, etc.)
4. Validate at each breakpoint
```

## Code Quality Standards

### Generated Code Should:

1. **Be production-ready**: No placeholder comments or TODOs
2. **Use semantic HTML**: Proper tags, ARIA attributes when needed
3. **Be accessible**: Follow WCAG guidelines
4. **Be maintainable**: Clear naming, proper structure
5. **Match design exactly**: Pixel-perfect implementation
6. **Be performant**: Optimized images, efficient CSS
7. **Be typed**: Use TypeScript when appropriate
8. **Be documented**: Clear comments for complex logic

### CSS/Styling Should:

1. Use exact values from Figma (don't round or approximate)
2. Follow the user's preferred approach (CSS Modules, Tailwind, styled-components, etc.)
3. Be responsive if design provides multiple breakpoints
4. Use design tokens/variables when available
5. Be organized and maintainable

## Debugging Tips

### Design Context Not Loading
```
- Verify fileKey and nodeId are correct
- Check authentication with whoami
- Ensure node exists and is visible in Figma
- Try get_metadata first to confirm access
```

### Generated Code Doesn't Match Design
```
- Call get_design_context again for fresh data
- Take a screenshot to visually compare
- Check if you're looking at the right nodeId
- Verify you specified the correct framework/language
```

### Missing Design Tokens
```
- Call get_variable_defs with the parent fileKey
- Variables may be defined at file level, not node level
- Check if design uses local styles vs variables
```

### Code Connect Not Working
```
- Verify Code Connect is published in Figma
- Check codeConnectLabel matches your framework
- Try disableCodeConnect=true to see raw design data
```

## Success Metrics

You've successfully used this skill when:
- ✅ Your implementation matches the Figma design pixel-perfectly
- ✅ You've called `get_design_context` multiple times during implementation
- ✅ You've validated each component individually before moving on
- ✅ You've used exact values from Figma (spacing, colors, dimensions)
- ✅ You've leveraged Code Connect when available
- ✅ You've extracted and used design tokens/variables
- ✅ Your code is production-ready and maintainable

## Related Skills

- **nextjs-seo**: Use after building the UI to optimize for search engines
- **clear-content**: Use when writing component documentation
- **design-system-setup** (future): Systematic design system creation from Figma

## Reference Files

- [MCP Tool Reference](./references/mcp-tools.md) - Detailed documentation of all Figma MCP tools
- [URL Parsing Guide](./references/url-parsing.md) - How to extract fileKey and nodeId from various Figma URL formats
- [Framework Examples](./references/framework-examples.md) - Code examples for React, Vue, Svelte, and more

---

**Important Reminders:**

1. 🔄 **Always iterate**: Call MCP tools multiple times, not just once at the start
2. ✅ **Always validate**: Compare after building each component
3. 🎯 **Always be exact**: Pixel-perfect means pixel-perfect, not close enough
4. 🔍 **Always refresh context**: Fetch fresh design data when comparing

The quality of your implementation depends on how well you follow the iterative comparison loop.

