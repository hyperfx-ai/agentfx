# Figma URL Parsing Guide

Complete guide for extracting `fileKey` and `nodeId` from all Figma URL formats.

## URL Format Overview

Figma uses different URL structures depending on the type of content:
- **Design files**: `/design/`
- **FigJam boards**: `/board/`
- **Prototype mode**: `/proto/`
- **Dev mode**: `/dev/`
- **Branch files**: `/branch/`

## Standard Design File URL

### Format
```
https://figma.com/design/:fileKey/:fileName?node-id=:int1-:int2
```

### Example
```
https://figma.com/design/pqrs/ExampleFile?node-id=1-2
```

### Extraction
```javascript
const url = "https://figma.com/design/pqrs/ExampleFile?node-id=1-2"

// Extract fileKey
const fileKey = "pqrs"  // From URL path

// Extract nodeId
const nodeId = "1:2"  // From query param, convert dash to colon

// Usage in MCP
get_design_context({
  fileKey: "pqrs",
  nodeId: "1:2"
})
```

### Rules
- **fileKey**: Text between `/design/` and the next `/`
- **nodeId**: Value of `node-id` query parameter
- **Convert dashes to colons**: `1-2` becomes `1:2`

---

## Branch URL

### Format
```
https://figma.com/design/:fileKey/branch/:branchKey/:fileName?node-id=:int1-:int2
```

### Example
```
https://figma.com/design/abcd/branch/xyz123/MyBranch?node-id=5-10
```

### Extraction
```javascript
const url = "https://figma.com/design/abcd/branch/xyz123/MyBranch?node-id=5-10"

// When branch exists, use branchKey as fileKey
const fileKey = "xyz123"  // The branchKey, NOT "abcd"
const nodeId = "5:10"     // From query param

// Usage in MCP
get_design_context({
  fileKey: "xyz123",  // Use branch key!
  nodeId: "5:10"
})
```

### Rules
- **fileKey**: Use the `branchKey` (after `/branch/`), NOT the main file key
- **nodeId**: Same as standard format
- Branch URLs take precedence over main file URLs

---

## FigJam Board URL

### Format
```
https://figma.com/board/:fileKey/:fileName?node-id=:int1-:int2
```

### Example
```
https://figma.com/board/board123/TeamBrainstorm?node-id=0-1
```

### Extraction
```javascript
const url = "https://figma.com/board/board123/TeamBrainstorm?node-id=0-1"

const fileKey = "board123"
const nodeId = "0:1"

// Usage: Use FigJam-specific tool
get_figjam({
  fileKey: "board123",
  nodeId: "0:1"
})
```

### Important
- FigJam URLs require `get_figjam`, NOT `get_design_context`
- `/board/` indicates it's a FigJam file

---

## Prototype Mode URL

### Format
```
https://figma.com/proto/:fileKey/:fileName?node-id=:int1-:int2
```

### Example
```
https://figma.com/proto/proto456/MyPrototype?node-id=10-20
```

### Extraction
```javascript
const url = "https://figma.com/proto/proto456/MyPrototype?node-id=10-20"

const fileKey = "proto456"
const nodeId = "10:20"

// Usage: Convert to design URL for MCP
get_design_context({
  fileKey: "proto456",
  nodeId: "10:20"
})
```

### Note
- Prototype URLs point to the same file as design URLs
- Use standard `get_design_context` (not a special tool)

---

## Dev Mode URL

### Format
```
https://figma.com/dev/:fileKey/:fileName?node-id=:int1-:int2
```

### Example  
```
https://figma.com/dev/dev789/DesignSystem?node-id=100-200
```

### Extraction
```javascript
const url = "https://figma.com/dev/dev789/DesignSystem?node-id=100-200"

const fileKey = "dev789"
const nodeId = "100:200"

get_design_context({
  fileKey: "dev789",
  nodeId: "100:200"
})
```

---

## URL Without node-id Parameter

### Example
```
https://figma.com/design/xyz789/HomePage
```

### Extraction
```javascript
const url = "https://figma.com/design/xyz789/HomePage"

const fileKey = "xyz789"
const nodeId = "0:1"  // Default to page root

// Get structure first
get_metadata({
  fileKey: "xyz789",
  nodeId: "0:1"
})
```

### Default Behavior
- When `node-id` is missing, default to `"0:1"` (usually the first page)
- Use `get_metadata` to see available nodes
- Ask user which specific node they want

---

## URL with Multiple Query Parameters

### Example
```
https://figma.com/design/abc/File?node-id=10-20&scaling=min-zoom&page-id=5%3A0
```

### Extraction
```javascript
const url = new URL("https://figma.com/design/abc/File?node-id=10-20&scaling=min-zoom&page-id=5%3A0")

const fileKey = "abc"
const nodeId = "10:20"  // Only extract node-id
const pageId = "5:0"    // Optional: page-id (decode %3A to :)

get_design_context({
  fileKey: "abc",
  nodeId: "10:20"
})
```

### Rules
- Extract only `node-id` parameter
- Ignore other parameters (`scaling`, `page-id`, etc.)
- URL-decode special characters (`%3A` becomes `:`)

---

## Node ID Format Rules

### Valid Formats

#### In URL (with dashes)
```
node-id=1-2
node-id=123-456
node-id=0-1
```

#### In MCP (with colons)
```
"1:2"
"123:456"
"0:1"
```

### Conversion
```javascript
// URL format → MCP format
function convertNodeId(urlNodeId) {
  return urlNodeId.replace(/-/g, ':')
}

// Examples
"1-2"     → "1:2"
"123-456" → "123:456"
"0-1"     → "0:1"
```

### Pattern
- URL uses **dashes**: `1-2`
- MCP uses **colons**: `1:2`
- Always convert before calling MCP tools

---

## Special Cases

### Case 1: Negative Node IDs

```
node-id=-5--10
```

This represents node ID `-5:-10`

```javascript
// Extract carefully
const nodeId = "-5:-10"  // Negative numbers are valid
```

### Case 2: Encoded Characters

```
node-id=1%3A2  // %3A is URL-encoded colon
```

```javascript
// Decode first
const decoded = decodeURIComponent("1%3A2")  // "1:2"
const nodeId = "1:2"
```

### Case 3: Missing Filename

```
https://figma.com/design/abc123?node-id=1-2
```

```javascript
// Still valid - filename is optional in URL
const fileKey = "abc123"
const nodeId = "1:2"
```

---

## Complete Parsing Function

```javascript
function parseFigmaUrl(url) {
  try {
    const urlObj = new URL(url)
    const pathParts = urlObj.pathname.split('/').filter(Boolean)
    
    // Determine file type
    const fileType = pathParts[0]  // 'design', 'board', 'proto', 'dev'
    
    // Extract fileKey
    let fileKey
    if (pathParts.includes('branch')) {
      // Branch URL: use branchKey
      const branchIndex = pathParts.indexOf('branch')
      fileKey = pathParts[branchIndex + 1]
    } else {
      // Standard URL: use fileKey
      fileKey = pathParts[1]
    }
    
    // Extract nodeId
    const nodeIdParam = urlObj.searchParams.get('node-id')
    const nodeId = nodeIdParam 
      ? nodeIdParam.replace(/-/g, ':')  // Convert dashes to colons
      : '0:1'  // Default to page root
    
    // Determine if FigJam
    const isFigJam = fileType === 'board'
    
    return {
      fileKey,
      nodeId,
      fileType,
      isFigJam,
      originalUrl: url
    }
  } catch (error) {
    throw new Error(`Invalid Figma URL: ${url}`)
  }
}

// Usage examples
parseFigmaUrl("https://figma.com/design/abc/File?node-id=1-2")
// → { fileKey: "abc", nodeId: "1:2", fileType: "design", isFigJam: false }

parseFigmaUrl("https://figma.com/design/abc/branch/xyz/Branch?node-id=5-10")
// → { fileKey: "xyz", nodeId: "5:10", fileType: "design", isFigJam: false }

parseFigmaUrl("https://figma.com/board/board123/Team?node-id=0-1")
// → { fileKey: "board123", nodeId: "0:1", fileType: "board", isFigJam: true }
```

---

## Validation Checklist

Before calling MCP tools, verify:

- ✅ **fileKey is extracted**: Not empty, not undefined
- ✅ **nodeId format is correct**: Uses colons (`:`) not dashes (`-`)
- ✅ **Branch handling**: Used branchKey for branch URLs
- ✅ **FigJam detection**: Used `get_figjam` for `/board/` URLs
- ✅ **URL is decoded**: No `%3A` or other encoded characters

---

## Common Mistakes

### ❌ Mistake 1: Using dashes in nodeId
```javascript
// Wrong
get_design_context({
  fileKey: "abc",
  nodeId: "1-2"  // ❌ Still has dashes
})

// Correct
get_design_context({
  fileKey: "abc",
  nodeId: "1:2"  // ✅ Converted to colons
})
```

### ❌ Mistake 2: Using main fileKey for branches
```javascript
// Wrong
// URL: figma.com/design/abc/branch/xyz/Branch
get_design_context({
  fileKey: "abc",  // ❌ Using main file key
  nodeId: "1:2"
})

// Correct
get_design_context({
  fileKey: "xyz",  // ✅ Using branch key
  nodeId: "1:2"
})
```

### ❌ Mistake 3: Using get_design_context for FigJam
```javascript
// Wrong
// URL: figma.com/board/abc/Board
get_design_context({  // ❌ Wrong tool for FigJam
  fileKey: "abc",
  nodeId: "0:1"
})

// Correct
get_figjam({  // ✅ Use FigJam tool
  fileKey: "abc",
  nodeId: "0:1"
})
```

---

## Testing Your Parser

### Test Cases

```javascript
const testUrls = [
  // Standard design file
  {
    url: "https://figma.com/design/abc/File?node-id=1-2",
    expected: { fileKey: "abc", nodeId: "1:2", isFigJam: false }
  },
  
  // Branch
  {
    url: "https://figma.com/design/abc/branch/xyz/Branch?node-id=5-10",
    expected: { fileKey: "xyz", nodeId: "5:10", isFigJam: false }
  },
  
  // FigJam
  {
    url: "https://figma.com/board/board123/Team?node-id=0-1",
    expected: { fileKey: "board123", nodeId: "0:1", isFigJam: true }
  },
  
  // No node-id
  {
    url: "https://figma.com/design/xyz/Page",
    expected: { fileKey: "xyz", nodeId: "0:1", isFigJam: false }
  },
  
  // Prototype mode
  {
    url: "https://figma.com/proto/proto456/Proto?node-id=10-20",
    expected: { fileKey: "proto456", nodeId: "10:20", isFigJam: false }
  }
]

// Run tests
testUrls.forEach(test => {
  const result = parseFigmaUrl(test.url)
  console.assert(
    result.fileKey === test.expected.fileKey &&
    result.nodeId === test.expected.nodeId &&
    result.isFigJam === test.expected.isFigJam,
    `Failed for ${test.url}`
  )
})
```

---

## Quick Reference

| URL Type | Pattern | fileKey Source | Special Notes |
|----------|---------|---------------|---------------|
| Design | `/design/:key/:name` | `:key` | Standard |
| Branch | `/design/:key/branch/:branch/:name` | `:branch` | Use branch key! |
| FigJam | `/board/:key/:name` | `:key` | Use `get_figjam` |
| Prototype | `/proto/:key/:name` | `:key` | Same as design |
| Dev Mode | `/dev/:key/:name` | `:key` | Same as design |

**Node ID Conversion**: Always `dash → colon` (`1-2` → `1:2`)

---

For tool usage examples, see [mcp-tools.md](./mcp-tools.md) and the main [SKILL.md](../SKILL.md).

