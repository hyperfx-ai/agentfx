# Figma MCP Tools Reference

Complete reference for all Figma Model Context Protocol (MCP) tools available in Claude and Cursor.

## Tool Categories

### 🎨 Design Extraction Tools
- `get_design_context` - Primary tool for design-to-code
- `get_metadata` - Structural overview
- `get_screenshot` - Visual screenshots

### 🎨 Design System Tools
- `get_variable_defs` - Extract design tokens
- `create_design_system_rules` - Generate design system documentation

### 🔗 Code Connect Tools
- `get_code_connect_map` - View Figma-to-code mappings
- `add_code_connect_map` - Create new mappings

### 📊 FigJam Tools
- `get_figjam` - Extract FigJam content
- `generate_diagram` - Create diagrams with Mermaid.js

### 🛠️ Utility Tools
- `whoami` - Check authentication

---

## Detailed Tool Documentation

### get_design_context

**The primary tool for Figma-to-code conversion.**

#### Purpose
Extracts complete design context from a Figma node and generates corresponding code based on your specified framework and language.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | Figma file key from URL |
| `nodeId` | string | ✅ Yes | Node ID (format: `123:456`) |
| `clientLanguages` | string | No | Comma-separated languages (e.g., `"typescript,javascript"`) |
| `clientFrameworks` | string | No | Comma-separated frameworks (e.g., `"react,tailwind"`) |
| `disableCodeConnect` | boolean | No | Set to `true` to disable Code Connect lookups |
| `forceCode` | boolean | No | Force code generation even for large outputs |

#### Returns
```typescript
{
  code: string,           // Generated code in specified framework
  assets: {              // Asset download URLs
    [assetId: string]: string
  },
  codeConnect?: {        // Code Connect information (if available)
    source: string,
    component: string
  },
  metadata: {            // Design metadata
    width: number,
    height: number,
    type: string
  }
}
```

#### When to Use
- **Primary use**: Getting code for any Figma node
- After building each component (to validate)
- When you need fresh design context
- When starting implementation

#### Best Practices
```typescript
// ✅ Good: Specify framework and language
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react",
  clientLanguages: "typescript"
})

// ❌ Bad: No framework specified
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25"
})

// ✅ Good: Multiple frameworks
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react,tailwind",
  clientLanguages: "typescript"
})
```

#### Common Issues

**Issue**: Code is generic HTML instead of React
**Solution**: Always specify `clientFrameworks: "react"` (or your framework)

**Issue**: Output says "too large"
**Solution**: Use `forceCode: true` or break down into smaller nodeIds

**Issue**: Not getting Code Connect data
**Solution**: Ensure Code Connect is published in Figma, or use `disableCodeConnect: false`

---

### get_metadata

**Gets structural overview of a Figma node in XML format.**

#### Purpose
Provides high-level structure without detailed styling. Use this to understand hierarchy and get child node IDs.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | Figma file key |
| `nodeId` | string | ✅ Yes | Node ID (can be page ID like `0:1`) |
| `clientLanguages` | string | No | For logging purposes |
| `clientFrameworks` | string | No | For logging purposes |

#### Returns
```xml
<node id="10:25" type="FRAME" name="Header" width="1440" height="80">
  <node id="10:26" type="TEXT" name="Logo" width="120" height="40"/>
  <node id="10:27" type="FRAME" name="Navigation" width="600" height="40">
    <node id="10:28" type="TEXT" name="Home" width="60" height="40"/>
    <node id="10:29" type="TEXT" name="About" width="60" height="40"/>
  </node>
</node>
```

#### When to Use
- Understanding overall page/component structure
- Getting child node IDs to process individually
- Navigating complex designs
- When file is too large for full design context

#### Workflow Example
```
1. Call get_metadata on page node (0:1)
   → See it has Header (10:25), Content (10:26), Footer (10:27)

2. Call get_design_context on each section individually
   → get_design_context(nodeId="10:25") for Header
   → get_design_context(nodeId="10:26") for Content
   → get_design_context(nodeId="10:27") for Footer
```

---

### get_screenshot

**Generates a visual screenshot of a Figma node.**

#### Purpose
Visual inspection and comparison between design and implementation.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | Figma file key |
| `nodeId` | string | ✅ Yes | Node ID to screenshot |
| `clientLanguages` | string | No | For logging |
| `clientFrameworks` | string | No | For logging |

#### Returns
Image data (displayed in chat)

#### When to Use
- Visually inspecting a design before coding
- Comparing your implementation to the design
- Showing the user what you're looking at
- Debugging visual mismatches

#### Best Practice
```
1. Take screenshot at start: "Here's what we're building"
2. Implement component
3. Take screenshot again: "Comparing against this"
4. Show user final result with screenshot
```

---

### get_variable_defs

**Extracts design tokens and variables from Figma.**

#### Purpose
Get color, spacing, typography, and other variables defined in Figma to use as design tokens in code.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | Figma file key |
| `nodeId` | string | ✅ Yes | Node ID |
| `clientLanguages` | string | No | For logging |
| `clientFrameworks` | string | No | For logging |

#### Returns
```json
{
  "color/primary/500": "#3B82F6",
  "color/gray/100": "#F3F4F6",
  "spacing/sm": "8px",
  "spacing/md": "16px",
  "font/size/body": "16px",
  "font/weight/bold": "700"
}
```

#### When to Use
- Setting up design tokens at project start
- Creating CSS variables or theme files
- Before hardcoding colors/spacing
- Building a design system

#### Implementation Example
```typescript
// 1. Get variables from Figma
const variables = get_variable_defs({
  fileKey: "abc123",
  nodeId: "0:1" // usually page or file root
})

// 2. Create CSS variables
:root {
  --color-primary-500: #3B82F6;
  --color-gray-100: #F3F4F6;
  --spacing-sm: 8px;
  --spacing-md: 16px;
}

// 3. Use in components
.button {
  background: var(--color-primary-500);
  padding: var(--spacing-md);
}
```

---

### get_code_connect_map

**Gets mapping of Figma nodes to existing code components.**

#### Purpose
Understand which Figma components are already linked to code components in your codebase.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | Figma file key |
| `nodeId` | string | ✅ Yes | Node ID |
| `codeConnectLabel` | string | No | Framework filter (e.g., "React", "Vue") |

#### Returns
```json
{
  "10:25": {
    "codeConnectSrc": "components/Button.tsx",
    "codeConnectName": "Button"
  },
  "10:26": {
    "codeConnectSrc": "components/Input.tsx",
    "codeConnectName": "Input"
  }
}
```

#### When to Use
- Before building components (check if they exist)
- Understanding component architecture
- Auditing Figma-to-code mappings
- Working with existing design systems

#### Workflow
```
1. User says: "Build this form"
2. Call get_code_connect_map to see what's already mapped
3. Found: Button and Input already exist
4. Only build: Form container and custom fields
5. Reuse existing Button and Input components
```

---

### add_code_connect_map

**Creates a mapping between a Figma node and a code component.**

#### Purpose
Document the connection between Figma designs and code for future reference and tooling.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | Figma file key |
| `nodeId` | string | ✅ Yes | Node ID to map |
| `source` | string | ✅ Yes | File path in codebase (e.g., `"src/components/Button.tsx"`) |
| `componentName` | string | ✅ Yes | Component name (e.g., `"Button"`) |
| `label` | string | ✅ Yes | Framework label (see options below) |

#### Label Options
- `"React"`
- `"Vue"`
- `"Svelte"`
- `"Web Components"`
- `"Storybook"`
- `"Javascript"`
- `"Swift UIKit"`
- `"Objective-C UIKit"`
- `"SwiftUI"`
- `"Compose"`
- `"Java"`
- `"Kotlin"`
- `"Android XML Layout"`
- `"Flutter"`

#### When to Use
- After creating a new component from Figma
- Setting up Code Connect for the first time
- Building design system documentation
- Establishing design-code links

#### Example
```typescript
// After building Button component from Figma
add_code_connect_map({
  fileKey: "abc123",
  nodeId: "10:25",
  source: "src/components/Button.tsx",
  componentName: "Button",
  label: "React"
})
```

---

### create_design_system_rules

**Generates design system rules for the repository.**

#### Purpose
Creates comprehensive design system documentation based on Figma variables and components.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clientLanguages` | string | No | Languages used in project |
| `clientFrameworks` | string | No | Frameworks used in project |

#### Returns
Markdown documentation with design system guidelines

#### When to Use
- Initializing a new design system
- Creating style guides
- Documenting design patterns
- Onboarding new developers

---

### get_figjam

**Extracts content from FigJam files (Figma's whiteboarding tool).**

#### Purpose
Get text, shapes, and structure from FigJam boards.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fileKey` | string | ✅ Yes | FigJam file key |
| `nodeId` | string | ✅ Yes | Node ID |
| `includeImagesOfNodes` | boolean | No | Include images (default: true) |
| `clientLanguages` | string | No | For logging |
| `clientFrameworks` | string | No | For logging |

#### Important
Only works with FigJam URLs: `https://figma.com/board/...`

#### When to Use
- Extracting brainstorming content
- Converting whiteboard flows to code
- Processing FigJam diagrams

---

### generate_diagram

**Creates diagrams in FigJam using Mermaid.js syntax.**

#### Purpose
Generate flowcharts, sequence diagrams, state diagrams, and gantt charts in FigJam.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | ✅ Yes | Diagram title |
| `mermaidSyntax` | string | ✅ Yes | Mermaid.js code |
| `userIntent` | string | No | Description of what user wants |

#### Supported Diagram Types
- `graph` / `flowchart`
- `sequenceDiagram`
- `stateDiagram` / `stateDiagram-v2`
- `gantt`

#### Example
```typescript
generate_diagram({
  name: "Authentication Flow",
  mermaidSyntax: `
    flowchart LR
      A["User"] --> B["Login"]
      B --> C{"Valid?"}
      C -->|"Yes"| D["Dashboard"]
      C -->|"No"| E["Error"]
  `
})
```

#### Returns
FigJam URL to view and edit the diagram

---

### whoami

**Returns information about the authenticated Figma user.**

#### Purpose
Debug authentication and permission issues.

#### Parameters
None

#### Returns
```json
{
  "id": "user123",
  "email": "user@example.com",
  "name": "John Doe"
}
```

#### When to Use
- Debugging: "Permission denied" errors
- Verifying: Correct user is authenticated
- Troubleshooting: MCP connection issues

---

## Tool Usage Patterns

### Pattern 1: Simple Component
```
1. get_design_context(nodeId) → Get code
2. Implement component
3. get_design_context(nodeId) → Validate
```

### Pattern 2: Complex Page
```
1. get_metadata(pageNode) → Get structure
2. For each section:
   a. get_design_context(sectionNode) → Get code
   b. Implement section
   c. get_design_context(sectionNode) → Validate
3. get_screenshot(pageNode) → Final check
```

### Pattern 3: Design System Setup
```
1. get_variable_defs() → Get tokens
2. Create theme/CSS variables
3. get_code_connect_map() → Check existing components
4. For each component:
   a. get_design_context(componentNode)
   b. Build component
   c. add_code_connect_map()
5. create_design_system_rules() → Document
```

### Pattern 4: Component with Variants
```
1. get_metadata(componentNode) → See variants
2. For each variant:
   a. get_design_context(variantNode)
3. Build component with variant logic
4. Validate each variant individually
```

---

## Common Error Messages

### "Node not found"
- **Cause**: Invalid nodeId or fileKey
- **Fix**: Check URL parsing, ensure node exists in Figma

### "Permission denied"
- **Cause**: Not authenticated or no file access
- **Fix**: Run `whoami`, ensure correct Figma account

### "Output too large"
- **Cause**: Node has too much content
- **Fix**: Use `forceCode: true` or break into smaller nodes

### "Code Connect not available"
- **Cause**: Code Connect not published in Figma
- **Fix**: Publish Code Connect or use `disableCodeConnect: true`

---

## Performance Tips

1. **Cache nodeIds**: Store child node IDs from `get_metadata` to avoid repeated calls

2. **Batch operations**: Process multiple components before validating all at once (if appropriate)

3. **Use Code Connect**: Leverage existing mappings to avoid regenerating code

4. **Strategic screenshots**: Take screenshots only when needed for visual comparison

5. **Targeted validation**: Validate changed components, not entire page every time

---

## Security Considerations

1. **Never hardcode credentials**: Use environment variables for API keys
2. **Validate URLs**: Ensure Figma URLs are legitimate before parsing
3. **Sanitize inputs**: Clean user-provided nodeIds and fileKeys
4. **Respect permissions**: Don't access files without proper authorization
5. **Keep MCP updated**: Use latest version to avoid security vulnerabilities

---

For workflow examples and practical implementations, see the main [SKILL.md](../SKILL.md) file.

