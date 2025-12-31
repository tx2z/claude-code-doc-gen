---
description: "[Internal] Component documentation agent - use /doc-gen instead"
disable-model-invocation: true
allowed-tools: Read, Glob, Grep, Write
---

# Component Documentation Agent (DOC06)

Generate comprehensive documentation for React, Vue, and Angular components including props documentation, usage examples, accessibility notes, and Storybook stories.

---

## 1. Component Discovery

### 1.1 Identify Component Framework

**React:**
```
Glob: **/*.tsx, **/*.jsx
Grep: import React|from 'react'|from "react"|React\.FC|React\.Component
```

**Vue:**
```
Glob: **/*.vue
Grep: <template>|<script>|defineComponent|setup\(\)
```

**Angular:**
```
Glob: **/*.component.ts
Grep: @Component|@Input|@Output
```

**Svelte:**
```
Glob: **/*.svelte
Grep: <script>|export let
```

### 1.2 Locate Components

**Search patterns by framework:**

**React components:**
```
Glob: src/components/**/*.tsx, components/**/*.tsx, app/components/**/*.tsx
Grep: export (default )?(function|const) [A-Z]|React\.FC|React\.Component
```

**Vue components:**
```
Glob: src/components/**/*.vue, components/**/*.vue
```

**Angular components:**
```
Glob: src/app/**/*.component.ts
Grep: @Component\(
```

### 1.3 Extract Component Information

**For each component, gather:**
- [ ] Component name
- [ ] Props/Inputs definition
- [ ] Events/Outputs
- [ ] Slots/Children
- [ ] Internal state
- [ ] Hooks/Composables used
- [ ] Styling approach
- [ ] Dependencies

---

## 2. React Component Documentation

### 2.1 Props Documentation

**Search for props definition:**
```
Grep: interface.*Props|type.*Props|PropTypes
```

**Generate props table:**

```markdown
## Props

| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| `variant` | `'primary' \| 'secondary' \| 'danger'` | `'primary'` | No | Visual style variant |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | No | Button size |
| `disabled` | `boolean` | `false` | No | Disable button interactions |
| `loading` | `boolean` | `false` | No | Show loading spinner |
| `onClick` | `(event: MouseEvent) => void` | - | No | Click event handler |
| `children` | `ReactNode` | - | Yes | Button content |
```

### 2.2 Component Documentation Template

```markdown
# Button

A customizable button component with multiple variants, sizes, and states.

## Import

```tsx
import { Button } from '@/components/Button';
```

## Props

| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| `variant` | `'primary' \| 'secondary' \| 'outline' \| 'ghost' \| 'danger'` | `'primary'` | No | Visual style variant |
| `size` | `'xs' \| 'sm' \| 'md' \| 'lg' \| 'xl'` | `'md'` | No | Button size |
| `disabled` | `boolean` | `false` | No | Disable interactions |
| `loading` | `boolean` | `false` | No | Show loading state |
| `fullWidth` | `boolean` | `false` | No | Expand to container width |
| `leftIcon` | `ReactNode` | - | No | Icon before label |
| `rightIcon` | `ReactNode` | - | No | Icon after label |
| `onClick` | `(e: MouseEvent) => void` | - | No | Click handler |
| `type` | `'button' \| 'submit' \| 'reset'` | `'button'` | No | HTML button type |
| `children` | `ReactNode` | - | Yes | Button content |

## Usage Examples

### Basic Usage

```tsx
<Button onClick={() => console.log('clicked')}>
  Click me
</Button>
```

### Variants

```tsx
<Button variant="primary">Primary</Button>
<Button variant="secondary">Secondary</Button>
<Button variant="outline">Outline</Button>
<Button variant="ghost">Ghost</Button>
<Button variant="danger">Danger</Button>
```

### Sizes

```tsx
<Button size="xs">Extra Small</Button>
<Button size="sm">Small</Button>
<Button size="md">Medium</Button>
<Button size="lg">Large</Button>
<Button size="xl">Extra Large</Button>
```

### With Icons

```tsx
import { PlusIcon, ArrowRightIcon } from '@heroicons/react/24/outline';

<Button leftIcon={<PlusIcon />}>
  Add Item
</Button>

<Button rightIcon={<ArrowRightIcon />}>
  Continue
</Button>
```

### Loading State

```tsx
const [loading, setLoading] = useState(false);

<Button
  loading={loading}
  onClick={async () => {
    setLoading(true);
    await saveData();
    setLoading(false);
  }}
>
  {loading ? 'Saving...' : 'Save'}
</Button>
```

### As Form Submit Button

```tsx
<form onSubmit={handleSubmit}>
  <input type="text" name="email" />
  <Button type="submit" loading={isSubmitting}>
    Submit
  </Button>
</form>
```

## Accessibility

- Uses native `<button>` element for proper semantics
- `disabled` state prevents focus and removes from tab order
- Loading state sets `aria-busy="true"`
- Supports keyboard navigation (Enter/Space to activate)
- Includes focus visible styles for keyboard users
- Icon-only buttons should include `aria-label`

```tsx
// Icon-only button with accessibility
<Button aria-label="Add new item">
  <PlusIcon />
</Button>
```

## Styling

The component uses Tailwind CSS classes and supports customization via:

### Custom className

```tsx
<Button className="custom-shadow">
  Custom Styled
</Button>
```

### CSS Variables

```css
:root {
  --button-primary-bg: theme('colors.blue.600');
  --button-primary-hover: theme('colors.blue.700');
  --button-border-radius: theme('borderRadius.lg');
}
```

## Related Components

- [IconButton](/docs/components/icon-button) - Icon-only button variant
- [ButtonGroup](/docs/components/button-group) - Group of related buttons
- [LinkButton](/docs/components/link-button) - Button styled as link
```

### 2.3 TypeScript Interface Documentation

```typescript
/**
 * Props for the Button component.
 */
export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  /**
   * Visual style variant of the button.
   * @default 'primary'
   */
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';

  /**
   * Size of the button affecting padding and font size.
   * @default 'md'
   */
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';

  /**
   * Whether the button is in a loading state.
   * When true, shows a spinner and disables interactions.
   * @default false
   */
  loading?: boolean;

  /**
   * Whether the button should take full width of its container.
   * @default false
   */
  fullWidth?: boolean;

  /**
   * Icon element to display before the button text.
   */
  leftIcon?: React.ReactNode;

  /**
   * Icon element to display after the button text.
   */
  rightIcon?: React.ReactNode;

  /**
   * Button content. Required for text buttons.
   * For icon-only buttons, provide aria-label instead.
   */
  children?: React.ReactNode;
}
```

---

## 3. Vue Component Documentation

### 3.1 Props Documentation (Vue)

**Search for props definition:**
```
Grep: defineProps|props:|@Prop\(
```

**Generate documentation:**

```markdown
# VButton

A customizable button component for Vue applications.

## Import

```vue
<script setup>
import { VButton } from '@/components/VButton.vue';
</script>
```

## Props

| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| `variant` | `'primary' \| 'secondary' \| 'danger'` | `'primary'` | No | Visual style |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | No | Button size |
| `disabled` | `boolean` | `false` | No | Disable button |
| `loading` | `boolean` | `false` | No | Loading state |

## Events

| Event | Payload | Description |
|-------|---------|-------------|
| `click` | `MouseEvent` | Emitted when button is clicked |
| `focus` | `FocusEvent` | Emitted when button receives focus |
| `blur` | `FocusEvent` | Emitted when button loses focus |

## Slots

| Slot | Props | Description |
|------|-------|-------------|
| `default` | - | Button content |
| `icon` | - | Icon before text |
| `loading` | - | Custom loading indicator |

## Usage

### Basic

```vue
<template>
  <VButton @click="handleClick">
    Click me
  </VButton>
</template>
```

### With Icon Slot

```vue
<template>
  <VButton>
    <template #icon>
      <PlusIcon />
    </template>
    Add Item
  </VButton>
</template>
```

### v-model (Toggle Button)

```vue
<template>
  <VButton v-model:pressed="isPressed">
    {{ isPressed ? 'On' : 'Off' }}
  </VButton>
</template>

<script setup>
import { ref } from 'vue';
const isPressed = ref(false);
</script>
```
```

### 3.2 Vue Component Script Documentation

```vue
<script setup lang="ts">
/**
 * VButton - A versatile button component.
 *
 * @example
 * <VButton variant="primary" @click="handleClick">
 *   Click me
 * </VButton>
 */

import { computed, type PropType } from 'vue';

/**
 * Available button variants determining visual appearance.
 */
type ButtonVariant = 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';

/**
 * Button size options affecting padding and font size.
 */
type ButtonSize = 'xs' | 'sm' | 'md' | 'lg' | 'xl';

const props = withDefaults(defineProps<{
  /**
   * Visual style variant of the button.
   */
  variant?: ButtonVariant;

  /**
   * Size of the button.
   */
  size?: ButtonSize;

  /**
   * Whether the button is disabled.
   */
  disabled?: boolean;

  /**
   * Whether the button is in loading state.
   */
  loading?: boolean;
}>(), {
  variant: 'primary',
  size: 'md',
  disabled: false,
  loading: false,
});

const emit = defineEmits<{
  /**
   * Emitted when the button is clicked.
   * @param event - The mouse event
   */
  click: [event: MouseEvent];
}>();

/**
 * Computed classes based on variant and size.
 */
const buttonClasses = computed(() => {
  return [
    'btn',
    `btn--${props.variant}`,
    `btn--${props.size}`,
    {
      'btn--disabled': props.disabled,
      'btn--loading': props.loading,
    },
  ];
});
</script>
```

---

## 4. Angular Component Documentation

### 4.1 Component Decorator Documentation

```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';

/**
 * A reusable button component with multiple variants and states.
 *
 * @example
 * ```html
 * <app-button
 *   variant="primary"
 *   size="md"
 *   (buttonClick)="onButtonClick()">
 *   Click me
 * </app-button>
 * ```
 *
 * @export
 * @class ButtonComponent
 */
@Component({
  selector: 'app-button',
  templateUrl: './button.component.html',
  styleUrls: ['./button.component.scss'],
})
export class ButtonComponent {
  /**
   * Visual style variant of the button.
   *
   * @type {('primary' | 'secondary' | 'danger')}
   * @default 'primary'
   */
  @Input() variant: 'primary' | 'secondary' | 'danger' = 'primary';

  /**
   * Size of the button.
   *
   * @type {('sm' | 'md' | 'lg')}
   * @default 'md'
   */
  @Input() size: 'sm' | 'md' | 'lg' = 'md';

  /**
   * Whether the button is disabled.
   *
   * @type {boolean}
   * @default false
   */
  @Input() disabled = false;

  /**
   * Whether the button shows a loading spinner.
   *
   * @type {boolean}
   * @default false
   */
  @Input() loading = false;

  /**
   * Event emitted when the button is clicked.
   * Only fires when button is not disabled or loading.
   */
  @Output() buttonClick = new EventEmitter<MouseEvent>();

  /**
   * Handles button click events.
   * @param event - The mouse event from the click
   */
  onClick(event: MouseEvent): void {
    if (!this.disabled && !this.loading) {
      this.buttonClick.emit(event);
    }
  }
}
```

---

## 5. Storybook Stories Generation

### 5.1 React Storybook Story

```typescript
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './Button';
import { PlusIcon, ArrowRightIcon } from '@heroicons/react/24/outline';

/**
 * Button component for user interactions.
 *
 * Supports multiple variants, sizes, and states including
 * loading and disabled states.
 */
const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'outline', 'ghost', 'danger'],
      description: 'Visual style variant',
      table: {
        defaultValue: { summary: 'primary' },
      },
    },
    size: {
      control: 'select',
      options: ['xs', 'sm', 'md', 'lg', 'xl'],
      description: 'Button size',
      table: {
        defaultValue: { summary: 'md' },
      },
    },
    disabled: {
      control: 'boolean',
      description: 'Disable button interactions',
    },
    loading: {
      control: 'boolean',
      description: 'Show loading spinner',
    },
    fullWidth: {
      control: 'boolean',
      description: 'Expand to full width',
    },
    onClick: { action: 'clicked' },
  },
  args: {
    children: 'Button',
    variant: 'primary',
    size: 'md',
    disabled: false,
    loading: false,
    fullWidth: false,
  },
};

export default meta;
type Story = StoryObj<typeof Button>;

/**
 * Default button with primary variant.
 */
export const Default: Story = {
  args: {
    children: 'Button',
  },
};

/**
 * All button variants displayed together.
 */
export const Variants: Story = {
  render: () => (
    <div className="flex gap-4">
      <Button variant="primary">Primary</Button>
      <Button variant="secondary">Secondary</Button>
      <Button variant="outline">Outline</Button>
      <Button variant="ghost">Ghost</Button>
      <Button variant="danger">Danger</Button>
    </div>
  ),
};

/**
 * All button sizes from extra small to extra large.
 */
export const Sizes: Story = {
  render: () => (
    <div className="flex items-center gap-4">
      <Button size="xs">Extra Small</Button>
      <Button size="sm">Small</Button>
      <Button size="md">Medium</Button>
      <Button size="lg">Large</Button>
      <Button size="xl">Extra Large</Button>
    </div>
  ),
};

/**
 * Button with icons on left or right side.
 */
export const WithIcons: Story = {
  render: () => (
    <div className="flex gap-4">
      <Button leftIcon={<PlusIcon className="w-4 h-4" />}>
        Add Item
      </Button>
      <Button rightIcon={<ArrowRightIcon className="w-4 h-4" />}>
        Continue
      </Button>
    </div>
  ),
};

/**
 * Button in loading state showing spinner.
 */
export const Loading: Story = {
  args: {
    loading: true,
    children: 'Loading...',
  },
};

/**
 * Disabled button that cannot be interacted with.
 */
export const Disabled: Story = {
  args: {
    disabled: true,
    children: 'Disabled',
  },
};

/**
 * Full width button expanding to container width.
 */
export const FullWidth: Story = {
  args: {
    fullWidth: true,
    children: 'Full Width Button',
  },
  decorators: [
    (Story) => (
      <div className="w-96">
        <Story />
      </div>
    ),
  ],
};

/**
 * Interactive playground to test all props.
 */
export const Playground: Story = {
  args: {
    children: 'Playground Button',
  },
};
```

### 5.2 Vue Storybook Story

```typescript
import type { Meta, StoryObj } from '@storybook/vue3';
import VButton from './VButton.vue';

const meta: Meta<typeof VButton> = {
  title: 'Components/VButton',
  component: VButton,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'outline', 'ghost', 'danger'],
    },
    size: {
      control: 'select',
      options: ['xs', 'sm', 'md', 'lg', 'xl'],
    },
    disabled: { control: 'boolean' },
    loading: { control: 'boolean' },
  },
  args: {
    default: 'Button',
  },
};

export default meta;
type Story = StoryObj<typeof VButton>;

export const Default: Story = {
  render: (args) => ({
    components: { VButton },
    setup() {
      return { args };
    },
    template: '<VButton v-bind="args">{{ args.default }}</VButton>',
  }),
  args: {
    variant: 'primary',
    size: 'md',
  },
};

export const Variants: Story = {
  render: () => ({
    components: { VButton },
    template: `
      <div class="flex gap-4">
        <VButton variant="primary">Primary</VButton>
        <VButton variant="secondary">Secondary</VButton>
        <VButton variant="outline">Outline</VButton>
        <VButton variant="ghost">Ghost</VButton>
        <VButton variant="danger">Danger</VButton>
      </div>
    `,
  }),
};
```

---

## 6. Accessibility Documentation

### 6.1 WCAG Compliance Checklist

```markdown
## Accessibility

This component follows WCAG 2.1 Level AA guidelines.

### Keyboard Navigation

| Key | Action |
|-----|--------|
| `Tab` | Move focus to button |
| `Shift + Tab` | Move focus from button |
| `Enter` | Activate button |
| `Space` | Activate button |

### Screen Reader Support

- Uses semantic `<button>` element
- `disabled` attribute properly disables and announces state
- Loading state announced via `aria-busy` and live region
- Icon-only buttons require `aria-label`

### Focus Management

- Visible focus ring meets 3:1 contrast ratio
- Focus is not trapped
- Focus order follows visual order

### Color Contrast

| State | Foreground | Background | Ratio |
|-------|------------|------------|-------|
| Primary | #FFFFFF | #2563EB | 4.63:1 |
| Primary Hover | #FFFFFF | #1D4ED8 | 5.91:1 |
| Disabled | #9CA3AF | #E5E7EB | 3.02:1 |

### Testing Checklist

- [ ] Navigable by keyboard only
- [ ] Announced correctly by VoiceOver
- [ ] Announced correctly by NVDA
- [ ] Visible focus indicator
- [ ] Sufficient color contrast
- [ ] Does not rely on color alone
- [ ] Touch target at least 44x44px
```

### 6.2 ARIA Attributes

```typescript
// Component with proper ARIA attributes
<button
  type={type}
  disabled={disabled || loading}
  aria-disabled={disabled || loading}
  aria-busy={loading}
  aria-label={ariaLabel}
  className={buttonClasses}
  onClick={handleClick}
>
  {loading && (
    <span
      className="loading-spinner"
      role="status"
      aria-label="Loading"
    />
  )}
  {children}
</button>
```

---

## 7. Test Scenarios

### 7.1 Test Cases Documentation

```markdown
## Test Scenarios

### Unit Tests

| Scenario | Expected Result |
|----------|-----------------|
| Renders with default props | Button displays with primary variant, md size |
| Applies variant class | Button has correct variant-specific styling |
| Handles click event | onClick callback is called with event |
| Disables when disabled prop | Button is not clickable, has disabled styling |
| Shows loading state | Spinner visible, button disabled |
| Renders children | Content is displayed inside button |
| Renders with icons | Icons appear in correct position |

### Integration Tests

| Scenario | Steps | Expected Result |
|----------|-------|-----------------|
| Form submission | Click submit button | Form submits, loading state shown |
| Disabled during async | Click, wait for completion | Button disabled during operation |
| Focus management | Tab to button | Visible focus indicator |

### Visual Regression Tests

- All variants at all sizes
- Hover states
- Focus states
- Loading state
- Disabled state
- RTL layout
- High contrast mode
```

### 7.2 Example Test Code

```typescript
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button')).toHaveTextContent('Click me');
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);

    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('does not call onClick when disabled', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick} disabled>Click me</Button>);

    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).not.toHaveBeenCalled();
  });

  it('shows loading spinner when loading', () => {
    render(<Button loading>Submit</Button>);
    expect(screen.getByRole('status')).toBeInTheDocument();
  });

  it('applies variant class', () => {
    render(<Button variant="danger">Delete</Button>);
    expect(screen.getByRole('button')).toHaveClass('btn--danger');
  });
});
```

---

## 8. Output Format

### 8.1 Component Documentation File

Generate documentation as MDX for each component:

```mdx
---
title: Button
description: A customizable button component with multiple variants and states.
---

import { Button } from '@/components/Button';

# Button

{{DESCRIPTION}}

<ComponentPreview>
  <Button>Example Button</Button>
</ComponentPreview>

## Import

{{IMPORT_STATEMENT}}

## Props

{{PROPS_TABLE}}

## Usage

{{USAGE_EXAMPLES}}

## Variants

{{VARIANTS_SHOWCASE}}

## Accessibility

{{ACCESSIBILITY_NOTES}}

## Related Components

{{RELATED_COMPONENTS}}
```

### 8.2 Storybook Story File

Generate `.stories.tsx` or `.stories.ts` files for each component following the patterns in Section 5.

---

## 9. Quality Checks

Before finalizing, verify:

- [ ] All props are documented with types and defaults
- [ ] Usage examples are working code
- [ ] Accessibility requirements are documented
- [ ] Keyboard navigation is specified
- [ ] Storybook stories cover all variants
- [ ] Test scenarios are comprehensive
- [ ] Related components are linked
- [ ] Import statements are correct
- [ ] TypeScript types are accurate
- [ ] Documentation builds without errors
