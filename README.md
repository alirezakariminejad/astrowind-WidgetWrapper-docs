# 🚀 AstroWind WidgetWrapper

A comprehensive guide to understanding and implementing the WidgetWrapper component in AstroWind projects.

## 📖 Introduction

The WidgetWrapper component is a versatile wrapper designed to provide a standardized structure for widgets in AstroWind. It offers features like dark mode support, customizable backgrounds, and responsive layouts.

## 🔧 Core Interfaces

```typescript
// Widget Interface
export interface Widget {
  id?: string;              // Optional widget identifier
  isDark?: boolean;         // Dark mode flag
  bg?: string;             // Background HTML/component
  classes?: Record<string, string | Record<string, string>>; // CSS classes object
}

// WidgetWrapper Props
export interface Props extends Widget {
  containerClass?: string;  // Additional container classes
  ['as']?: HTMLTag;        // HTML element type
}
```

# 🔍 Code Structure Breakdown

## 📦 Component Organization

### 1. Imports Section

```typescript
import type { HTMLTag } from 'astro/types';
import type { Widget } from '~/types';
import { twMerge } from 'tailwind-merge';
import Background from './Background.astro';
```

#### Key Imports
- `HTMLTag`: TypeScript type for HTML element tags from Astro
- `Widget`: Custom interface for widget properties
- `twMerge`: Utility for merging Tailwind CSS classes efficiently
- `Background`: Default background component

### 2. Interface Definition

```typescript
export interface Props extends Widget {
  containerClass?: string; // Optional class for container styling
  ['as']?: HTMLTag;        // Optional HTML element type
}
```

#### Interface Details
- Extends the base `Widget` interface
- `containerClass`: Additional classes for container customization
- `['as']`: Dynamic HTML element type specification

### 3. Props Destructuring

```typescript
const {
  id,               // Widget identifier
  isDark = false,   // Dark mode flag (default: false)
  containerClass = '', // Container classes (default: empty)
  bg,              // Custom background
  as = 'section'   // HTML element (default: 'section')
} = Astro.props;

const WrapperTag = as; // Dynamic element variable
```

### 4. Component Template Structure

#### Main Wrapper
```astro
<WrapperTag
  class="relative not-prose scroll-mt-[72px]"
  {...id ? { id } : {}}
>
```
- Dynamic HTML element (`WrapperTag`)
- Base positioning and scroll margin
- Conditional ID attribute

#### Background Layer
```astro
<div
  class="absolute inset-0 pointer-events-none -z-[1]"
  aria-hidden="true"
>
  <slot name="bg">
    {bg ? <Fragment set:html={bg} /> : <Background isDark={isDark} />}
  </slot>
</div>
```
- Absolute positioning for background
- Prevents pointer events
- Negative z-index for layering
- Named slot for custom background
- Fallback to default Background component

#### Content Container
```astro
<div
  class:list={[
    twMerge(
      'relative mx-auto max-w-7xl px-4 md:px-6 py-12 md:py-16 lg:py-20 text-default',
      containerClass
    ),
    { dark: isDark },
  ]}
>
  <slot />
</div>
```
- Responsive padding and sizing
- Maximum width constraint
- Dark mode toggle
- Default slot for content

## ✨ Key Features Explained

### 1. Dynamic Element Rendering
```astro
const WrapperTag = as;
<WrapperTag>...</WrapperTag>
```
- Allows switching between different HTML elements
- Maintains semantic HTML structure
- Useful for SEO optimization

### 2. Slot System
```astro
<slot name="bg">
  {/* Background content */}
</slot>
<slot /> // Default slot
```
- Named slot for background customization
- Default slot for main content
- Enables component composition

### 3. Class Management
```typescript
twMerge(
  'relative mx-auto max-w-7xl px-4 md:px-6 py-12 md:py-16 lg:py-20 text-default',
  containerClass
)
```
- Efficient class merging
- Responsive design classes
- Custom class integration

### 4. Background Handling
```astro
{bg ? <Fragment set:html={bg} /> : <Background isDark={isDark} />}
```
- Custom background support
- Default background fallback
- Dark mode integration

## 🛠️ Technical Notes

1. **Component Flexibility**
   - The component is designed to be highly adaptable through props
   - Supports both static and dynamic content
   - Easy integration with existing design systems

2. **Performance Considerations**
   - Efficient class merging with `twMerge`
   - Minimal DOM nesting
   - Optimized background rendering

3. **Accessibility Features**
   - Semantic HTML structure
   - ARIA attributes for background elements
   - Support for dark mode preferences

## 📚 Usage Best Practices

1. **Element Selection**
   ```astro
   <WidgetWrapper as="main"> // For main content
   <WidgetWrapper as="article"> // For blog posts
   <WidgetWrapper as="section"> // For general sections
   ```

2. **Background Customization**
   ```astro
   <WidgetWrapper bg={customBackgroundHtml}>
   ```

3. **Container Styling**
   ```astro
   <WidgetWrapper containerClass="custom-class another-class">
   ```

## 🔄 Component Lifecycle

1. Props initialization and validation
2. Dynamic element type assignment
3. Background preparation
4. Content rendering
5. Class merging and application

### Basic Structure

```astro
<WrapperTag class="relative not-prose scroll-mt-[72px]" {...id ? { id } : {}}>
  <div class="absolute inset-0 pointer-events-none -z-[1]" aria-hidden="true">
    <slot name="bg">
      {bg ? <Fragment set:html={bg} /> : <Background isDark={isDark} />}
    </slot>
  </div>
  <div
    class:list={[
      twMerge('relative mx-auto max-w-7xl px-4 md:px-6 py-12 md:py-16 lg:py-20 text-default', containerClass),
      { dark: isDark },
    ]}
  >
    <slot />
  </div>
</WrapperTag>
```

## 💡 Usage Examples

### Basic Implementation

```astro
---
import WidgetWrapper from './WidgetWrapper.astro';
---

<WidgetWrapper id="basic-example">
  <div class="space-y-4">
    <h2 class="text-2xl font-bold">Feature Section</h2>
    <p>Your content goes here</p>
  </div>
</WidgetWrapper>
```

### Advanced Implementation with Custom Background

```astro
---
import WidgetWrapper from './WidgetWrapper.astro';

const widgetConfig: Widget = {
  id: "advanced-example",
  isDark: true,
  bg: `<div class="absolute inset-0 bg-gradient-to-r from-blue-500 to-purple-500 opacity-50"></div>`,
  classes: {
    container: {
      default: "bg-white shadow-lg rounded-lg",
      dark: "bg-gray-800 text-white"
    }
  }
};
---

<WidgetWrapper {...widgetConfig} as="section" containerClass="py-8">
  <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
    <div class="space-y-4">
      <h2 class="text-3xl font-bold">Main Title</h2>
      <p class="text-lg">Description text goes here</p>
      <button class="bg-blue-500 text-white px-6 py-2 rounded-lg">
        Call to Action
      </button>
    </div>
  </div>
</WidgetWrapper>
```

### Responsive Layout Example

```astro
<WidgetWrapper
  id="responsive-example"
  containerClass="px-4 sm:px-6 lg:px-8 py-8 sm:py-12 lg:py-16"
>
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    <div class="p-6 bg-white rounded-lg shadow-md">
      <h3 class="text-xl font-semibold mb-4">Feature 1</h3>
      <p>Feature description</p>
    </div>
    <!-- Repeat for other features -->
  </div>
</WidgetWrapper>
```

## ✨ Key Features

### 1. Flexible Styling
- Built-in dark mode support
- Customizable container classes
- Responsive design patterns

### 2. Background Management
- Custom background slot
- Default background component
- z-index management

### 3. Semantic HTML
- Configurable HTML element type
- SEO-friendly structure
- Accessibility considerations

## 🎯 Best Practices

### 1. Use Semantic Elements

```astro
<WidgetWrapper as="article" id="blog-post">
  <h1>Title</h1>
  <p>Content</p>
</WidgetWrapper>
```

### 2. Organize Classes

```typescript
const classes = {
  container: {
    default: "bg-white shadow-lg",
    dark: "bg-gray-800"
  },
  content: {
    default: "space-y-4",
    dark: "space-y-6"
  }
};
```

### 3. Handle Dark Mode

```astro
<WidgetWrapper
  isDark={true}
  containerClass="rounded-xl"
>
  <div class="space-y-6">
    <h2 class="text-2xl font-bold">Dark Mode Section</h2>
    <p class="text-gray-300">Content with automatic dark mode styling</p>
  </div>
</WidgetWrapper>
```

## 📝 Common Use Cases

### 1. Landing Page Sections

```astro
<WidgetWrapper id="hero">
  <!-- Hero content -->
</WidgetWrapper>

<WidgetWrapper id="features" isDark={true}>
  <!-- Features grid -->
</WidgetWrapper>
```

### 2. Blog Posts

```astro
<WidgetWrapper as="article" containerClass="prose lg:prose-xl">
  <!-- Blog content -->
</WidgetWrapper>
```

### 3. Interactive Components

```astro
<WidgetWrapper 
  id="pricing"
  containerClass="hover:shadow-xl transition-shadow duration-300"
>
  <!-- Pricing tables -->
</WidgetWrapper>
```

## 🔍 Technical Considerations

### 1. Performance
- Minimal DOM nesting
- Efficient class merging with tailwind-merge
- Optimized background rendering

### 2. Accessibility
- Semantic HTML structure
- ARIA attributes
- Color contrast in dark mode

### 3. Maintainability
- Consistent class patterns
- Modular background system
- Clear prop interface

## 🤝 Contributing

Feel free to contribute to this documentation by creating a pull request or opening an issue. We welcome any improvements or suggestions!

## 📄 License

This documentation is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.