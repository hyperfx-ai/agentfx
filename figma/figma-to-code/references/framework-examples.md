# Framework-Specific Examples

Practical examples of using Figma MCP with different frameworks and languages.

## Table of Contents

- [React + TypeScript](#react--typescript)
- [React + Tailwind CSS](#react--tailwind-css)
- [Next.js](#nextjs)
- [Vue 3 + TypeScript](#vue-3--typescript)
- [Svelte](#svelte)
- [HTML + CSS](#html--css)
- [React Native](#react-native)
- [Flutter](#flutter)

---

## React + TypeScript

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react",
  clientLanguages: "typescript"
})
```

### Example Output
```typescript
// Button.tsx
import React from 'react';
import styles from './Button.module.css';

interface ButtonProps {
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  children: React.ReactNode;
  onClick?: () => void;
}

export const Button: React.FC<ButtonProps> = ({ 
  variant = 'primary',
  size = 'md',
  children,
  onClick 
}) => {
  return (
    <button 
      className={`${styles.button} ${styles[variant]} ${styles[size]}`}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
```

### Validation Workflow
```typescript
// 1. Get initial context
const context1 = get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react",
  clientLanguages: "typescript"
})

// 2. Implement Button component (as above)

// 3. Validate by fetching again
const context2 = get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react",
  clientLanguages: "typescript"
})

// 4. Compare:
// - Check padding: 12px 24px ✓
// - Check border-radius: 8px ✓
// - Check font-size: 16px ✓
// - Check colors match ✓
```

### Design Tokens Setup
```typescript
// 1. Get variables
get_variable_defs({
  fileKey: "abc123",
  nodeId: "0:1"
})

// 2. Create tokens file
// tokens.ts
export const tokens = {
  colors: {
    primary: {
      500: '#3B82F6',
      600: '#2563EB',
    },
    gray: {
      100: '#F3F4F6',
      900: '#111827',
    }
  },
  spacing: {
    sm: '8px',
    md: '16px',
    lg: '24px',
  },
  borderRadius: {
    sm: '4px',
    md: '8px',
    lg: '12px',
  }
} as const;

// 3. Use in components
import { tokens } from './tokens';

const buttonStyles = {
  backgroundColor: tokens.colors.primary[500],
  padding: `${tokens.spacing.sm} ${tokens.spacing.md}`,
  borderRadius: tokens.borderRadius.md,
};
```

---

## React + Tailwind CSS

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react,tailwind",
  clientLanguages: "typescript"
})
```

### Example Output
```typescript
// Button.tsx
import React from 'react';
import { cva, type VariantProps } from 'class-variance-authority';

const buttonVariants = cva(
  'inline-flex items-center justify-center rounded-lg font-medium transition-colors',
  {
    variants: {
      variant: {
        primary: 'bg-blue-500 text-white hover:bg-blue-600',
        secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
      },
      size: {
        sm: 'px-3 py-1.5 text-sm',
        md: 'px-4 py-2 text-base',
        lg: 'px-6 py-3 text-lg',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
);

interface ButtonProps 
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {}

export const Button: React.FC<ButtonProps> = ({ 
  variant, 
  size, 
  className,
  children,
  ...props 
}) => {
  return (
    <button 
      className={buttonVariants({ variant, size, className })}
      {...props}
    >
      {children}
    </button>
  );
};
```

### Tailwind Config from Figma
```typescript
// 1. Get design tokens
get_variable_defs({
  fileKey: "abc123",
  nodeId: "0:1"
})

// 2. Update tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          500: '#3B82F6',
          600: '#2563EB',
        },
        gray: {
          100: '#F3F4F6',
          200: '#E5E7EB',
          900: '#111827',
        }
      },
      spacing: {
        '18': '4.5rem',  // Custom spacing from Figma
      },
      borderRadius: {
        'xl': '12px',
      }
    }
  }
}
```

---

## Next.js

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react,nextjs",
  clientLanguages: "typescript"
})
```

### Example: Next.js 15 with App Router
```typescript
// app/components/Hero.tsx
import Image from 'next/image';

interface HeroProps {
  title: string;
  subtitle: string;
  imageUrl: string;
}

export function Hero({ title, subtitle, imageUrl }: HeroProps) {
  return (
    <section className="relative h-screen flex items-center justify-center">
      <Image
        src={imageUrl}
        alt={title}
        fill
        priority
        className="object-cover"
      />
      <div className="relative z-10 text-center text-white">
        <h1 className="text-6xl font-bold mb-4">{title}</h1>
        <p className="text-xl">{subtitle}</p>
      </div>
    </section>
  );
}
```

### Handling Figma Assets
```typescript
// 1. Get design context (includes asset URLs)
const context = get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react,nextjs",
  clientLanguages: "typescript"
})

// 2. Assets are returned in context.assets
// {
//   "image-123": "https://figma.com/file/abc/images/image-123.png",
//   "icon-456": "https://figma.com/file/abc/images/icon-456.svg"
// }

// 3. Download assets to public folder
// public/images/hero-background.png
// public/images/logo.svg

// 4. Reference in Next.js
<Image src="/images/hero-background.png" alt="Hero" width={1920} height={1080} />
```

---

## Vue 3 + TypeScript

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "vue",
  clientLanguages: "typescript"
})
```

### Example Output
```vue
<!-- Button.vue -->
<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'primary',
  size: 'md'
});

const buttonClasses = computed(() => {
  return [
    'button',
    `button--${props.variant}`,
    `button--${props.size}`
  ];
});

const emit = defineEmits<{
  click: [event: MouseEvent]
}>();
</script>

<template>
  <button 
    :class="buttonClasses"
    @click="emit('click', $event)"
  >
    <slot />
  </button>
</template>

<style scoped>
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s;
}

.button--primary {
  background-color: #3B82F6;
  color: white;
}

.button--primary:hover {
  background-color: #2563EB;
}

.button--secondary {
  background-color: #E5E7EB;
  color: #111827;
}

.button--sm {
  padding: 6px 12px;
  font-size: 14px;
}

.button--md {
  padding: 8px 16px;
  font-size: 16px;
}

.button--lg {
  padding: 12px 24px;
  font-size: 18px;
}
</style>
```

---

## Svelte

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "svelte",
  clientLanguages: "typescript"
})
```

### Example Output
```svelte
<!-- Button.svelte -->
<script lang="ts">
  export let variant: 'primary' | 'secondary' = 'primary';
  export let size: 'sm' | 'md' | 'lg' = 'md';
  
  let className = '';
  export { className as class };
  
  $: classes = [
    'button',
    `button--${variant}`,
    `button--${size}`,
    className
  ].filter(Boolean).join(' ');
</script>

<button class={classes} on:click>
  <slot />
</button>

<style>
  .button {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    border: none;
    border-radius: 8px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s;
  }
  
  .button--primary {
    background-color: #3B82F6;
    color: white;
  }
  
  .button--primary:hover {
    background-color: #2563EB;
  }
  
  .button--secondary {
    background-color: #E5E7EB;
    color: #111827;
  }
  
  .button--sm {
    padding: 6px 12px;
    font-size: 14px;
  }
  
  .button--md {
    padding: 8px 16px;
    font-size: 16px;
  }
  
  .button--lg {
    padding: 12px 24px;
    font-size: 18px;
  }
</style>
```

---

## HTML + CSS

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientLanguages: "html,css"
})
```

### Example Output
```html
<!-- button.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="button.css">
  <title>Button Component</title>
</head>
<body>
  <button class="button button--primary button--md">
    Click Me
  </button>
  
  <button class="button button--secondary button--sm">
    Cancel
  </button>
</body>
</html>
```

```css
/* button.css */
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  border-radius: 8px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.button--primary {
  background-color: #3B82F6;
  color: #FFFFFF;
}

.button--primary:hover {
  background-color: #2563EB;
}

.button--primary:active {
  background-color: #1D4ED8;
}

.button--secondary {
  background-color: #E5E7EB;
  color: #111827;
}

.button--secondary:hover {
  background-color: #D1D5DB;
}

.button--sm {
  padding: 6px 12px;
  font-size: 14px;
  line-height: 20px;
}

.button--md {
  padding: 8px 16px;
  font-size: 16px;
  line-height: 24px;
}

.button--lg {
  padding: 12px 24px;
  font-size: 18px;
  line-height: 28px;
}

.button:focus-visible {
  outline: 2px solid #3B82F6;
  outline-offset: 2px;
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
```

---

## React Native

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react",
  clientLanguages: "typescript"
})
```

### Example Output
```typescript
// Button.tsx
import React from 'react';
import { 
  TouchableOpacity, 
  Text, 
  StyleSheet, 
  ViewStyle,
  TextStyle 
} from 'react-native';

interface ButtonProps {
  variant?: 'primary' | 'secondary';
  size?: 'sm' | 'md' | 'lg';
  onPress?: () => void;
  children: string;
}

export const Button: React.FC<ButtonProps> = ({ 
  variant = 'primary',
  size = 'md',
  onPress,
  children 
}) => {
  return (
    <TouchableOpacity 
      style={[
        styles.button,
        styles[variant],
        styles[size]
      ]}
      onPress={onPress}
      activeOpacity={0.8}
    >
      <Text style={[styles.text, styles[`${variant}Text`]]}>
        {children}
      </Text>
    </TouchableOpacity>
  );
};

const styles = StyleSheet.create({
  button: {
    alignItems: 'center',
    justifyContent: 'center',
    borderRadius: 8,
  },
  primary: {
    backgroundColor: '#3B82F6',
  },
  secondary: {
    backgroundColor: '#E5E7EB',
  },
  sm: {
    paddingVertical: 6,
    paddingHorizontal: 12,
  },
  md: {
    paddingVertical: 8,
    paddingHorizontal: 16,
  },
  lg: {
    paddingVertical: 12,
    paddingHorizontal: 24,
  },
  text: {
    fontWeight: '500',
  },
  primaryText: {
    color: '#FFFFFF',
    fontSize: 16,
  },
  secondaryText: {
    color: '#111827',
    fontSize: 16,
  },
});
```

---

## Flutter

### MCP Call
```typescript
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "flutter",
  clientLanguages: "dart"
})
```

### Example Output
```dart
// button.dart
import 'package:flutter/material.dart';

enum ButtonVariant { primary, secondary }
enum ButtonSize { sm, md, lg }

class CustomButton extends StatelessWidget {
  final String text;
  final ButtonVariant variant;
  final ButtonSize size;
  final VoidCallback? onPressed;

  const CustomButton({
    Key? key,
    required this.text,
    this.variant = ButtonVariant.primary,
    this.size = ButtonSize.md,
    this.onPressed,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return ElevatedButton(
      onPressed: onPressed,
      style: ElevatedButton.styleFrom(
        backgroundColor: _getBackgroundColor(),
        foregroundColor: _getTextColor(),
        padding: _getPadding(),
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(8),
        ),
        elevation: 0,
      ),
      child: Text(
        text,
        style: TextStyle(
          fontSize: _getFontSize(),
          fontWeight: FontWeight.w500,
        ),
      ),
    );
  }

  Color _getBackgroundColor() {
    switch (variant) {
      case ButtonVariant.primary:
        return const Color(0xFF3B82F6);
      case ButtonVariant.secondary:
        return const Color(0xFFE5E7EB);
    }
  }

  Color _getTextColor() {
    switch (variant) {
      case ButtonVariant.primary:
        return Colors.white;
      case ButtonVariant.secondary:
        return const Color(0xFF111827);
    }
  }

  EdgeInsets _getPadding() {
    switch (size) {
      case ButtonSize.sm:
        return const EdgeInsets.symmetric(horizontal: 12, vertical: 6);
      case ButtonSize.md:
        return const EdgeInsets.symmetric(horizontal: 16, vertical: 8);
      case ButtonSize.lg:
        return const EdgeInsets.symmetric(horizontal: 24, vertical: 12);
    }
  }

  double _getFontSize() {
    switch (size) {
      case ButtonSize.sm:
        return 14;
      case ButtonSize.md:
        return 16;
      case ButtonSize.lg:
        return 18;
    }
  }
}

// Usage
CustomButton(
  text: 'Click Me',
  variant: ButtonVariant.primary,
  size: ButtonSize.md,
  onPressed: () {
    print('Button pressed');
  },
)
```

---

## Best Practices Across All Frameworks

### 1. Always Specify Framework and Language
```typescript
// ✅ Good
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25",
  clientFrameworks: "react,tailwind",
  clientLanguages: "typescript"
})

// ❌ Bad: Will get generic HTML
get_design_context({
  fileKey: "abc123",
  nodeId: "10:25"
})
```

### 2. Extract Design Tokens First
```typescript
// Before building components
get_variable_defs({ fileKey: "abc123", nodeId: "0:1" })

// Then create theme/tokens file
// Then build components using tokens
```

### 3. Validate with Screenshots
```typescript
// 1. Build component
// 2. Take screenshot of Figma
get_screenshot({ fileKey: "abc123", nodeId: "10:25" })

// 3. Compare visually
// 4. Fix mismatches
```

### 4. Use Code Connect for Consistency
```typescript
// After building Button.tsx
add_code_connect_map({
  fileKey: "abc123",
  nodeId: "10:25",
  source: "components/Button.tsx",
  componentName: "Button",
  label: "React"
})

// Later, check mappings
get_code_connect_map({ fileKey: "abc123", nodeId: "10:25" })
```

---

## Framework-Specific Considerations

### React/Next.js
- Use TypeScript for better type safety
- Extract reusable hooks for complex logic
- Use CSS Modules or Tailwind for styling
- Optimize images with Next.js Image component

### Vue
- Use Composition API for better TypeScript support
- Extract composables for shared logic
- Use scoped styles
- Consider Pinia for state management

### Svelte
- Leverage reactive declarations ($:)
- Use TypeScript in script tags
- Take advantage of built-in transitions
- Keep components small and focused

### React Native
- Use StyleSheet.create for performance
- Consider responsive units (not just px)
- Test on both iOS and Android
- Use Platform-specific code when needed

### Flutter
- Follow Flutter widget patterns
- Use const constructors when possible
- Leverage Theme for consistency
- Consider responsive layouts (MediaQuery)

---

For more details on MCP tools, see [mcp-tools.md](./mcp-tools.md).

