# Design System Reference

> **For AI tools (Claude Code, Cursor, Codex) and developers.**
> This file is the single source of truth for design tokens, component conventions, and project architecture.
> Source: Figma Design System file. Do not edit token values here — update the Figma file and re-export.

---

## Table of Contents

1. [Project Architecture](#project-architecture)
2. [Token Override Strategy](#token-override-strategy)
3. [Color Tokens](#color-tokens)
4. [Typography](#typography)
5. [Spacing Scale](#spacing-scale)
6. [Border Radius](#border-radius)
7. [Widths & Containers](#widths--containers)
8. [Elevation (Shadows)](#elevation-shadows)
9. [Backdrop Blur](#backdrop-blur)
10. [Component Guidelines](#component-guidelines)
11. [Dark Mode](#dark-mode)
12. [Creating New Components](#creating-new-components)
13. [File Naming Conventions](#file-naming-conventions)

---

## Project Architecture

```
your-project/
├── DESIGN_SYSTEM.md              ← This file (AI reads first)
├── styles/
│   ├── tokens/
│   │   ├── base.css              ← Base design system tokens (from Figma)
│   │   ├── base-components.css   ← Component-specific tokens (from Figma)
│   │   └── overrides.css         ← Project-specific token overrides
│   ├── base/                     ← Base component styles (matching Figma components)
│   │   ├── button.css
│   │   ├── input.css
│   │   ├── modal.css
│   │   └── ...
│   ├── custom/                   ← Project-specific new components
│   │   ├── dashboard-card.css
│   │   └── ...
│   └── main.css                  ← Import order entry point
├── components/                   ← Component markup (React/Vue/Svelte/etc.)
│   ├── base/                     ← Components matching Figma design system
│   └── custom/                   ← Project-specific components
└── ...
```

### Import Order (Critical)

```css
/* main.css — Order matters: later files override earlier ones */
@import 'tokens/base.css';              /* 1. Base design system tokens */
@import 'tokens/base-components.css';   /* 2. Component-specific tokens */
@import 'tokens/overrides.css';         /* 3. Project overrides */
@import 'base/index.css';              /* 4. Base component styles */
@import 'custom/index.css';            /* 5. Project-specific components */
```

---

## Token Override Strategy

### Rules

1. **Never modify `base.css` or `base-components.css`** — these are generated from Figma and will be overwritten on re-export.
2. **All project customizations go in `overrides.css`** — override only the tokens you need to change.
3. **New components go in `styles/custom/` and `components/custom/`** — never add project-specific components to the `base/` directories.
4. **Always use CSS custom properties** — never hardcode color values, spacing, or typography. Reference the token.
5. **Semantic tokens over primitives** — use `--color-bg-primary` not `--gray-50`. Semantic tokens automatically handle dark mode.

### Example Override

```css
/* overrides.css */
:root {
  /* Change brand color from purple to blue */
  --brand-25: #f0f5ff;
  --brand-50: #e0eaff;
  --brand-100: #c7d7fe;
  --brand-200: #a4bcfd;
  --brand-300: #8098f9;
  --brand-400: #6172f3;
  --brand-500: #444ce7;
  --brand-600: #3538cd;
  --brand-700: #2d31a6;
  --brand-800: #2d3282;
  --brand-900: #292b6b;
  --brand-950: #1f235b;

  /* Override specific semantic tokens */
  --color-bg-brand-solid: var(--brand-600);
  --color-bg-brand-solid-hover: var(--brand-700);

  /* Change font family */
  --font-family-display: 'Plus Jakarta Sans', sans-serif;
  --font-family-body: 'Plus Jakarta Sans', sans-serif;
}
```

---

## Color Tokens

### Primitive Color Ramps

These are the raw color values. Do not use directly in components — use semantic tokens instead.

```css
:root {
  /* --- Gray Ramp --- */
  --gray-25: #fdfdfd;
  --gray-50: #fafafa;
  --gray-100: #f5f5f5;
  --gray-200: #e9eaeb;
  --gray-300: #d5d7da;
  --gray-400: #a4a7ae;
  --gray-500: #717680;
  --gray-600: #535862;
  --gray-700: #414651;
  --gray-800: #252b37;
  --gray-900: #181d27;
  --gray-950: #0a0d12;

  /* --- Brand Ramp (default: Purple) --- */
  --brand-25: #fcfaff;
  --brand-50: #f9f5ff;
  --brand-100: #f4ebff;
  --brand-200: #e9d7fe;
  --brand-300: #d6bbfb;
  --brand-400: #b692f6;
  --brand-500: #9e77ed;
  --brand-600: #7f56d9;
  --brand-700: #6941c6;
  --brand-800: #53389e;
  --brand-900: #42307d;
  --brand-950: #2c1c5f;

  /* --- Error Ramp --- */
  --error-25: #fffbfa;
  --error-50: #fef3f2;
  --error-100: #fee4e2;
  --error-200: #fecdca;
  --error-300: #fda29b;
  --error-400: #f97066;
  --error-500: #f04438;
  --error-600: #d92d20;
  --error-700: #b42318;
  --error-800: #912018;
  --error-900: #7a271a;
  --error-950: #55160c;

  /* --- Warning Ramp --- */
  --warning-25: #fffcf5;
  --warning-50: #fffaeb;
  --warning-100: #fef0c7;
  --warning-200: #fedf89;
  --warning-300: #fec84b;
  --warning-400: #fdb022;
  --warning-500: #f79009;
  --warning-600: #dc6803;
  --warning-700: #b54708;
  --warning-800: #93370d;
  --warning-900: #7a2e0e;
  --warning-950: #4e1d09;

  /* --- Success Ramp --- */
  --success-25: #f6fef9;
  --success-50: #ecfdf3;
  --success-100: #dcfae6;
  --success-200: #abefc6;
  --success-300: #75e0a7;
  --success-400: #47cd89;
  --success-500: #17b26a;
  --success-600: #079455;
  --success-700: #067647;
  --success-800: #085d3a;
  --success-900: #074d31;
  --success-950: #053321;

  /* --- Base --- */
  --base-white: #ffffff;
  --base-black: #000000;
}
```

### Semantic Color Tokens (Light Mode)

These are the tokens you should use in components. They automatically switch in dark mode.

```css
:root {
  /* --- Backgrounds --- */
  --color-bg-primary: #ffffff;
  --color-bg-primary-hover: #fafafa;
  --color-bg-primary-alt: #ffffff;
  --color-bg-secondary: #fafafa;
  --color-bg-secondary-hover: #f5f5f5;
  --color-bg-secondary-subtle: #fdfdfd;
  --color-bg-secondary-solid: #535862;
  --color-bg-tertiary: #f5f5f5;
  --color-bg-quaternary: #e9eaeb;
  --color-bg-active: #fafafa;
  --color-bg-disabled: #f5f5f5;
  --color-bg-disabled-subtle: #fafafa;
  --color-bg-overlay: #0a0d12;
  --color-bg-brand-primary: #f9f5ff;
  --color-bg-brand-secondary: #f4ebff;
  --color-bg-brand-solid: #7f56d9;
  --color-bg-brand-solid-hover: #6941c6;
  --color-bg-brand-section: #53389e;
  --color-bg-brand-section-subtle: #6941c6;
  --color-bg-error-primary: #fef3f2;
  --color-bg-error-secondary: #fee4e2;
  --color-bg-error-solid: #d92d20;
  --color-bg-warning-primary: #fffaeb;
  --color-bg-warning-secondary: #fef0c7;
  --color-bg-warning-solid: #dc6803;
  --color-bg-success-primary: #ecfdf3;
  --color-bg-success-secondary: #dcfae6;
  --color-bg-success-solid: #079455;
  --color-bg-primary-solid: #0a0d12;

  /* --- Text --- */
  --color-text-primary: #181d27;
  --color-text-secondary: #414651;
  --color-text-secondary-hover: #252b37;
  --color-text-tertiary: #535862;
  --color-text-tertiary-hover: #414651;
  --color-text-quaternary: #717680;
  --color-text-disabled: #717680;
  --color-text-placeholder: #717680;
  --color-text-placeholder-subtle: #d5d7da;
  --color-text-white: #ffffff;
  --color-text-brand-primary: #42307d;
  --color-text-brand-secondary: #6941c6;
  --color-text-brand-tertiary: #7f56d9;
  --color-text-error-primary: #d92d20;
  --color-text-warning-primary: #dc6803;
  --color-text-success-primary: #079455;
  --color-text-primary-on-brand: #ffffff;
  --color-text-secondary-on-brand: #e9d7fe;
  --color-text-tertiary-on-brand: #e9d7fe;
  --color-text-quaternary-on-brand: #d6bbfb;

  /* --- Foreground (Icons & Indicators) --- */
  --color-fg-primary: #181d27;
  --color-fg-secondary: #414651;
  --color-fg-secondary-hover: #252b37;
  --color-fg-tertiary: #535862;
  --color-fg-tertiary-hover: #414651;
  --color-fg-quaternary: #717680;
  --color-fg-quaternary-hover: #535862;
  --color-fg-quinary: #a4a7ae;
  --color-fg-quinary-hover: #717680;
  --color-fg-senary: #d5d7da;
  --color-fg-white: #ffffff;
  --color-fg-disabled: #a4a7ae;
  --color-fg-disabled-subtle: #d5d7da;
  --color-fg-brand-primary: #7f56d9;
  --color-fg-brand-secondary: #9e77ed;
  --color-fg-error-primary: #d92d20;
  --color-fg-error-secondary: #f04438;
  --color-fg-warning-primary: #dc6803;
  --color-fg-warning-secondary: #f79009;
  --color-fg-success-primary: #079455;
  --color-fg-success-secondary: #17b26a;

  /* --- Borders --- */
  --color-border-primary: #d5d7da;
  --color-border-secondary: #e9eaeb;
  --color-border-tertiary: #f5f5f5;
  --color-border-disabled: #d5d7da;
  --color-border-disabled-subtle: #e9eaeb;
  --color-border-brand: #9e77ed;
  --color-border-brand-alt: #7f56d9;
  --color-border-error: #f04438;
  --color-border-error-subtle: #fda29b;

  /* --- Effects --- */
  --color-focus-ring: #9e77ed;
  --color-focus-ring-error: #f04438;
}
```

---

## Typography

### Font Families

```css
:root {
  --font-family-display: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-family-body: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}
```

### Font Weights

```css
:root {
  --font-weight-regular: 400;
  --font-weight-medium: 500;
  --font-weight-semibold: 600;
  --font-weight-bold: 700;
}
```

### Type Scale

| Token | Size | Line Height | Use Case |
|-------|------|-------------|----------|
| `--font-size-display-2xl` | 72px | 90px | Hero headlines |
| `--font-size-display-xl` | 60px | 72px | Major page headings |
| `--font-size-display-lg` | 48px | 60px | Page titles |
| `--font-size-display-md` | 36px | 44px | Section headings |
| `--font-size-display-sm` | 30px | 38px | Subsection headings, card titles |
| `--font-size-display-xs` | 24px | 32px | Small headings, widget headers |
| `--font-size-text-xl` | 20px | 30px | Lead paragraphs, featured text |
| `--font-size-text-lg` | 18px | 28px | Large body text, form section labels |
| `--font-size-text-md` | 16px | 24px | **Default body text**, form labels, UI text |
| `--font-size-text-sm` | 14px | 20px | Secondary text, table cells, captions |
| `--font-size-text-xs` | 12px | 18px | Fine print, badges, timestamps |

```css
:root {
  --font-size-display-2xl: 4.5rem;    /* 72px */
  --font-size-display-xl: 3.75rem;    /* 60px */
  --font-size-display-lg: 3rem;        /* 48px */
  --font-size-display-md: 2.25rem;     /* 36px */
  --font-size-display-sm: 1.875rem;    /* 30px */
  --font-size-display-xs: 1.5rem;      /* 24px */
  --font-size-text-xl: 1.25rem;        /* 20px */
  --font-size-text-lg: 1.125rem;       /* 18px */
  --font-size-text-md: 1rem;           /* 16px */
  --font-size-text-sm: 0.875rem;       /* 14px */
  --font-size-text-xs: 0.75rem;        /* 12px */

  --line-height-display-2xl: 5.625rem; /* 90px */
  --line-height-display-xl: 4.5rem;    /* 72px */
  --line-height-display-lg: 3.75rem;   /* 60px */
  --line-height-display-md: 2.75rem;   /* 44px */
  --line-height-display-sm: 2.375rem;  /* 38px */
  --line-height-display-xs: 2rem;      /* 32px */
  --line-height-text-xl: 1.875rem;     /* 30px */
  --line-height-text-lg: 1.75rem;      /* 28px */
  --line-height-text-md: 1.5rem;       /* 24px */
  --line-height-text-sm: 1.25rem;      /* 20px */
  --line-height-text-xs: 1.125rem;     /* 18px */
}
```

---

## Spacing Scale

Use for padding, margin, gap, and all dimensional spacing.

```css
:root {
  --spacing-none: 0px;
  --spacing-xxs: 2px;     /* 0.125rem */
  --spacing-xs: 4px;      /* 0.25rem */
  --spacing-sm: 6px;      /* 0.375rem */
  --spacing-md: 8px;      /* 0.5rem */
  --spacing-lg: 12px;     /* 0.75rem */
  --spacing-xl: 16px;     /* 1rem */
  --spacing-2xl: 20px;    /* 1.25rem */
  --spacing-3xl: 24px;    /* 1.5rem */
  --spacing-4xl: 32px;    /* 2rem */
  --spacing-5xl: 40px;    /* 2.5rem */
  --spacing-6xl: 48px;    /* 3rem */
  --spacing-7xl: 64px;    /* 4rem */
  --spacing-8xl: 80px;    /* 5rem */
  --spacing-9xl: 96px;    /* 6rem */
  --spacing-10xl: 128px;  /* 8rem */
  --spacing-11xl: 160px;  /* 10rem */
}
```

**Usage guidance:**
- `xxs–sm` — Compact UI: icon margins, tight padding, inline gaps
- `md–lg` — Standard components: input padding, card internal spacing
- `xl–3xl` — Component spacing: gaps between form fields, card padding
- `4xl–6xl` — Section spacing: gaps between content blocks
- `7xl–11xl` — Page sections: hero padding, section margins

---

## Border Radius

```css
:root {
  --radius-none: 0px;
  --radius-xxs: 2px;
  --radius-xs: 4px;
  --radius-sm: 6px;
  --radius-md: 8px;
  --radius-lg: 10px;
  --radius-xl: 12px;
  --radius-2xl: 16px;
  --radius-3xl: 20px;
  --radius-4xl: 24px;
  --radius-full: 9999px;   /* Pill shape / circle */
}
```

**Usage guidance:**
- `xxs–xs` — Subtle rounding: badges, small tags
- `sm–md` — Standard: buttons, inputs, cards
- `lg–xl` — Prominent: modals, large cards, image containers
- `2xl–4xl` — Decorative: hero sections, feature cards
- `full` — Pills: pill buttons, avatar circles, toggle tracks

---

## Widths & Containers

```css
:root {
  /* --- Max-Width Constraints --- */
  --width-xxs: 320px;
  --width-xs: 384px;
  --width-sm: 480px;
  --width-md: 560px;
  --width-lg: 640px;
  --width-xl: 768px;
  --width-2xl: 1024px;
  --width-3xl: 1280px;
  --width-4xl: 1440px;
  --width-5xl: 1600px;
  --width-6xl: 1920px;
  --width-paragraph-max: 720px;   /* Optimal line length (~65-75 chars) */

  /* --- Container Layout --- */
  --container-max-width: 1280px;
  --container-padding-desktop: 32px;
  --container-padding-mobile: 16px;
}
```

---

## Elevation (Shadows)

```css
:root {
  --shadow-xs: 0px 1px 2px 0px rgba(10, 13, 18, 0.05);
  --shadow-sm: 0px 1px 2px -1px rgba(10, 13, 18, 0.1), 0px 1px 3px 0px rgba(10, 13, 18, 0.1);
  --shadow-md: 0px 2px 4px -2px rgba(10, 13, 18, 0.06), 0px 4px 6px -1px rgba(10, 13, 18, 0.1);
  --shadow-lg: 0px 2px 2px -1px rgba(10, 13, 18, 0.04), 0px 4px 6px -2px rgba(10, 13, 18, 0.03), 0px 12px 16px -4px rgba(10, 13, 18, 0.08);
  --shadow-xl: 0px 3px 3px -1.5px rgba(10, 13, 18, 0.04), 0px 8px 8px -4px rgba(10, 13, 18, 0.03), 0px 20px 24px -4px rgba(10, 13, 18, 0.08);
  --shadow-2xl: 0px 4px 4px -2px rgba(10, 13, 18, 0.04), 0px 24px 48px -12px rgba(10, 13, 18, 0.18);
  --shadow-3xl: 0px 5px 5px -2.5px rgba(10, 13, 18, 0.04), 0px 32px 64px -12px rgba(10, 13, 18, 0.14);

  /* Skeuomorphic (for buttons with 3D effect) */
  --shadow-xs-skeuomorphic: 0px 1px 2px 0px rgba(10, 13, 18, 0.05), inset 0px -2px 0px 0px rgba(10, 13, 18, 0.05), inset 0px 0px 0px 1px rgba(10, 13, 18, 0.18);
}
```

| Token | Use Case |
|-------|----------|
| `shadow-xs` | Input fields, subtle card borders |
| `shadow-sm` | Cards, dropdowns, popovers |
| `shadow-md` | Floating cards, tooltips |
| `shadow-lg` | Modals, drawers, slide-out panels |
| `shadow-xl` | Critical overlays, command palettes |
| `shadow-2xl` | Top-level floating elements |
| `shadow-3xl` | Dramatic floating effects, showcases |

---

## Backdrop Blur

```css
:root {
  --backdrop-blur-sm: 8px;
  --backdrop-blur-md: 16px;
  --backdrop-blur-lg: 24px;
  --backdrop-blur-xl: 40px;
}
```

Usage: `backdrop-filter: blur(var(--backdrop-blur-md));`

---

## Component Guidelines

### Base Components (from Figma Design System)

These components exist in the Figma file and should have matching code implementations:

**Shared / Form:**
Button, Badge, Tag, Input, Textarea, Select/Dropdown, Toggle, Checkbox, Checkbox Group, Slider, Avatar, Avatar Group, Tooltip, Progress Bar, Progress Circle

**Application:**
Sidebar Navigation, Header Navigation, Modal, Command Bar, Table, Tabs (Horizontal/Vertical), Breadcrumbs, Alert, Notification, Date Picker, Calendar, File Upload, Pagination, Progress Steps, Activity Feed, Message, Content Divider, Loading Indicator, Empty State, Code Snippet, Metric Item, Charts (Line, Bar, Pie, Radar, Activity Gauge), Slide-out Menu, Inline CTA

**Marketing:**
Header Navigation (Full-width, Dropdown), Hero Section, Features Section, Pricing Section, CTA Section, Metrics Section, Newsletter CTA, Testimonial Section, Social Proof Section, Blog Section, Blog Post Card, Contact Section, Team Section, Careers Section, FAQ Section, Footer, Banner

### Component Properties (Variant System)

Most base components follow this variant property pattern from Figma:

| Property | Purpose | Common Values |
|----------|---------|---------------|
| `Size` | Component size | `sm`, `md`, `lg`, `xl`, `2xl` |
| `Hierarchy` | Visual prominence | `primary`, `secondary`, `tertiary`, `link` |
| `Color` | Semantic color | `brand`, `gray`, `error`, `warning`, `success` |
| `State` | Interaction state | `default`, `hover`, `focus`, `disabled` |
| `Type` | Layout variant | Component-specific |
| `Breakpoint` | Responsive variant | `Desktop`, `Mobile`, `Tablet` |
| `Destructive` | Danger/delete mode | `true`, `false` |

### Responsive Breakpoints

```css
/* Mobile first approach */
/* Mobile: default (< 768px) */
/* Tablet: 768px+ */
@media (min-width: 768px) { ... }
/* Desktop: 1024px+ */
@media (min-width: 1024px) { ... }
/* Wide: 1280px+ */
@media (min-width: 1280px) { ... }
```

---

## Dark Mode

### Implementation

The design system uses CSS custom properties that swap values in dark mode:

```css
/* Dark mode via class (recommended) */
.dark {
  --color-bg-primary: #0c0e12;
  --color-bg-primary-hover: #22262f;
  --color-bg-secondary: #13161b;
  --color-bg-secondary-hover: #22262f;
  --color-bg-tertiary: #22262f;
  --color-bg-quaternary: #373a41;
  --color-bg-disabled: #22262f;

  --color-text-primary: #f7f7f7;
  --color-text-secondary: #cecfd2;
  --color-text-tertiary: #94979c;
  --color-text-quaternary: #94979c;
  --color-text-disabled: #85888e;
  --color-text-placeholder: #85888e;
  --color-text-brand-secondary: #cecfd2;
  --color-text-error-primary: #f97066;
  --color-text-warning-primary: #fdb022;
  --color-text-success-primary: #47cd89;

  --color-fg-primary: #ffffff;
  --color-fg-secondary: #cecfd2;
  --color-fg-secondary-hover: #ececed;
  --color-fg-tertiary: #94979c;
  --color-fg-quaternary: #94979c;
  --color-fg-quinary: #85888e;
  --color-fg-senary: #61656c;
  --color-fg-disabled: #85888e;
  --color-fg-disabled-subtle: #61656c;
  --color-fg-brand-primary: #9e77ed;
  --color-fg-brand-secondary: #9e77ed;
  --color-fg-error-primary: #f04438;
  --color-fg-error-secondary: #f97066;
  --color-fg-warning-primary: #f79009;
  --color-fg-warning-secondary: #fdb022;
  --color-fg-success-primary: #17b26a;
  --color-fg-success-secondary: #47cd89;

  --color-border-primary: #373a41;
  --color-border-secondary: #22262f;
  --color-border-tertiary: #22262f;
  --color-border-disabled: #373a41;
  --color-border-disabled-subtle: #22262f;
  --color-border-brand: #b692f6;
  --color-border-error: #f97066;
  --color-border-error-subtle: #f97066;

  --color-bg-brand-solid: #7f56d9;
  --color-bg-brand-solid-hover: #9e77ed;
  --color-bg-brand-primary: #9e77ed;
  --color-bg-brand-secondary: #7f56d9;
  --color-bg-error-primary: #f04438;
  --color-bg-error-secondary: #d92d20;
  --color-bg-error-solid: #d92d20;
  --color-bg-warning-primary: #f79009;
  --color-bg-warning-secondary: #dc6803;
  --color-bg-success-primary: #17b26a;
  --color-bg-success-secondary: #079455;
}

/* Alternative: system preference */
@media (prefers-color-scheme: dark) {
  :root {
    /* Same dark mode tokens as above */
  }
}
```

### Dark Mode Rules

1. Never use `--gray-*` primitives directly — always use `--color-bg-*`, `--color-text-*`, `--color-border-*` semantic tokens.
2. Shadows become transparent in dark mode (the Figma system zeroes them out). Consider using borders or subtle background shifts for elevation in dark mode instead.
3. Brand colors stay consistent across modes. Error/warning/success shift to lighter variants in dark mode for visibility.

---

## Creating New Components

When building project-specific components that don't exist in the base design system:

### Checklist

1. **File location:** `styles/custom/[component-name].css` and `components/custom/[ComponentName].*`
2. **Use base tokens:** All colors, spacing, radius, typography, and shadows must use CSS custom properties from the token files.
3. **Follow naming convention:** CSS class names use `kebab-case`. Component files use `PascalCase`.
4. **Support dark mode:** Use semantic color tokens only. If your component needs a new color, add a semantic token to `overrides.css`, not a hardcoded value.
5. **Responsive:** Build mobile-first. Use the standard breakpoints (768px, 1024px, 1280px).
6. **Match the variant pattern:** If the component has sizes, use `sm/md/lg/xl`. If it has states, use `default/hover/focus/disabled`.

### Example: Custom Dashboard Card

```css
/* styles/custom/dashboard-card.css */
.dashboard-card {
  background: var(--color-bg-primary);
  border: 1px solid var(--color-border-secondary);
  border-radius: var(--radius-xl);
  padding: var(--spacing-3xl);
  box-shadow: var(--shadow-sm);
  transition: box-shadow 0.2s ease, border-color 0.2s ease;
}

.dashboard-card:hover {
  box-shadow: var(--shadow-md);
  border-color: var(--color-border-primary);
}

.dashboard-card__title {
  font-family: var(--font-family-display);
  font-size: var(--font-size-text-lg);
  font-weight: var(--font-weight-semibold);
  line-height: var(--line-height-text-lg);
  color: var(--color-text-primary);
  margin-bottom: var(--spacing-xs);
}

.dashboard-card__description {
  font-family: var(--font-family-body);
  font-size: var(--font-size-text-sm);
  line-height: var(--line-height-text-sm);
  color: var(--color-text-tertiary);
}

/* Size variants */
.dashboard-card--sm { padding: var(--spacing-xl); }
.dashboard-card--lg { padding: var(--spacing-4xl); }
```

---

## File Naming Conventions

| Type | Pattern | Example |
|------|---------|--------|
| Token files | `kebab-case.css` | `base.css`, `overrides.css` |
| Component CSS | `kebab-case.css` | `dashboard-card.css` |
| Component files | `PascalCase.*` | `DashboardCard.tsx` |
| Custom token variables | `--[category]-[name]` | `--color-bg-custom-surface` |
| CSS classes | `kebab-case` | `.dashboard-card` |
| BEM modifiers | `--[modifier]` | `.dashboard-card--sm` |
| BEM elements | `__[element]` | `.dashboard-card__title` |

---

## Quick Reference: What Goes Where

| Scenario | Where |
|----------|-------|
| Change brand colors for this project | `overrides.css` — override `--brand-*` ramp |
| Change font family | `overrides.css` — override `--font-family-*` |
| Adjust a specific component's token | `overrides.css` — override the semantic token |
| Build a new component | `styles/custom/` + `components/custom/` |
| Fix a bug in a base component | `styles/base/` (but flag for upstream sync) |
| Add a new color that doesn't exist | `overrides.css` — add new semantic token |
| Update base tokens from Figma | Re-export `base.css` and `base-components.css` |
