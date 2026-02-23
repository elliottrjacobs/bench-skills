# Token Architecture

Naming conventions, tier model, multi-brand strategies, and output formats for design tokens. Sources: W3C Design Tokens spec, Figma Schema 2025, Brad Frost, industry best practices.

---

## Token Tiers — The 3-Layer Model

Tokens are organized in three tiers, each adding meaning:

### Tier 1: Global (Raw Values)

The raw palette. No semantic meaning — just the value.

```
color.blue.100: #DBEAFE
color.blue.500: #3B82F6
color.blue.900: #1E3A8A
color.gray.50:  #F9FAFB
color.gray.900: #111827
font.size.xs:   12px
font.size.sm:   14px
font.size.md:   16px
space.1:        4px
space.2:        8px
space.4:        16px
```

### Tier 2: Semantic (Purpose-Mapped)

Maps purpose to global tokens. This is where theming happens.

```
color.primary:     {color.blue.500}
color.on-primary:  {color.white}
color.surface:     {color.gray.50}
color.on-surface:  {color.gray.900}
color.error:       {color.red.500}
color.success:     {color.green.500}
text.body:         {font.size.md}
text.heading:      {font.size.xl}
spacing.inline:    {space.2}
spacing.stack:     {space.4}
```

### Tier 3: Component (Scoped)

Tokens scoped to specific components. Reference semantic tokens.

```
button.primary.bg:       {color.primary}
button.primary.text:     {color.on-primary}
button.primary.hover.bg: {color.blue.600}
input.border:            {color.gray.300}
input.focus.border:      {color.primary}
card.bg:                 {color.surface}
card.shadow:             {elevation.sm}
```

**Why 3 tiers?** Changing `color.primary` from blue to purple cascades through every semantic and component token that references it. One change, full consistency.

---

## Naming Conventions

Pattern: `{category}.{property}.{variant}.{state}`

| Segment | What it is | Examples |
|---------|-----------|----------|
| **category** | Token domain | `color`, `font`, `space`, `radius`, `elevation`, `motion`, `z` |
| **property** | Specific attribute | `primary`, `body`, `inline`, `sm`, `fast` |
| **variant** | Sub-variation | `light`, `muted`, `inverse` |
| **state** | Interactive state | `hover`, `active`, `focus`, `disabled` |

**Rules:**
- Use dot notation for hierarchy: `color.primary.hover`
- Use kebab-case for multi-word segments: `color.on-primary`, `font.line-height`
- Scale with numbers: `space.1` through `space.12`, `font.size.xs` through `font.size.3xl`
- Keep names descriptive but short — `color.error` not `color.error-state-background-fill`

---

## Token Categories

| Category | What it covers | Example tokens |
|----------|---------------|----------------|
| **Color** | Palette, semantic colors, surfaces, text, borders | `color.primary`, `color.surface`, `color.border` |
| **Typography** | Font family, weight, size scale, line height, letter spacing | `font.family.sans`, `font.size.md`, `font.weight.bold` |
| **Spacing** | Padding, margin, gap (based on a base unit) | `space.1` (4px), `space.4` (16px) |
| **Sizing** | Component dimensions, icon sizes | `size.icon.sm`, `size.avatar.md` |
| **Border radius** | Corner rounding | `radius.sm`, `radius.full` |
| **Elevation** | Box shadows, depth | `elevation.sm`, `elevation.lg` |
| **Motion** | Duration, easing curves | `duration.fast`, `easing.ease-out` |
| **Breakpoints** | Responsive thresholds | `breakpoint.sm: 640px`, `breakpoint.lg: 1024px` |
| **Z-index** | Stacking order | `z.dropdown: 100`, `z.modal: 200`, `z.toast: 300` |
| **Opacity** | Transparency levels | `opacity.disabled: 0.5`, `opacity.overlay: 0.7` |

---

## Accessibility Tokens

Build accessibility into the token layer, not as an afterthought:

**Color contrast:**
- Document which foreground/background pairings meet WCAG AA (4.5:1 text, 3:1 large text)
- Semantic tokens like `color.on-primary` must always contrast against `color.primary` at AA minimum
- Provide a contrast audit table with every semantic color pairing

**Focus indicators:**
```
focus.ring.width:  2px
focus.ring.color:  {color.primary}
focus.ring.offset: 2px
focus.ring.style:  solid
```

**Motion / reduced motion:**
```
duration.normal:   200ms
duration.slow:     400ms
duration.reduced:  0ms      # Used when prefers-reduced-motion is active
```

**Touch targets:**
```
target.min.mobile:  44px    # iOS HIG minimum
target.min.touch:   48px    # Material / Android minimum
```

---

## Multi-Brand Architecture

For systems supporting multiple brands or themes:

**Structure:** Core system + brand overrides

```
tokens/
├── core/           # Shared foundations (spacing, radius, motion, z-index)
├── brand-a/        # Brand A overrides (color palette, typography)
├── brand-b/        # Brand B overrides
└── modes/
    ├── light.json  # Light mode semantic mappings
    └── dark.json   # Dark mode semantic mappings
```

**What's brandable vs. fixed:**
- **Brandable:** Color palette, font family, certain radius/shadow values
- **Fixed (usually):** Spacing scale, z-index, breakpoints, motion, accessibility tokens

**Override strategy:**
1. Start from core tokens (shared across all brands)
2. Brand layer overrides global tokens (new palette, new fonts)
3. Semantic tokens reference brand globals (so `color.primary` resolves to brand-specific value)
4. Component tokens stay the same (they reference semantic tokens)

---

## Theme Support (Light / Dark / High-Contrast)

Themes swap the **semantic layer** while keeping global and component layers intact:

```
# Light mode
color.surface:     {color.white}
color.on-surface:  {color.gray.900}
color.border:      {color.gray.200}

# Dark mode
color.surface:     {color.gray.900}
color.on-surface:  {color.gray.100}
color.border:      {color.gray.700}

# High-contrast mode
color.surface:     {color.black}
color.on-surface:  {color.white}
color.border:      {color.white}
```

This is why the semantic tier exists — components don't need to know which theme is active.

---

## W3C Design Tokens Format

The [W3C Design Tokens Community Group](https://design-tokens.github.io/community-group/format/) defines a JSON format for interoperability:

```json
{
  "color": {
    "primary": {
      "$value": "#3B82F6",
      "$type": "color",
      "$description": "Primary brand color"
    },
    "error": {
      "$value": "#EF4444",
      "$type": "color"
    }
  },
  "spacing": {
    "sm": {
      "$value": "8px",
      "$type": "dimension"
    }
  }
}
```

Use this format when tokens need to be consumed by multiple tools or platforms.

---

## Output Formats

Tokens can be expressed in multiple formats depending on the consumer:

| Format | When to use | Example |
|--------|------------|---------|
| **Markdown (docs)** | Documentation, human reference | `color.primary: #3B82F6` |
| **CSS custom properties** | Web projects | `--color-primary: #3B82F6;` |
| **JSON (W3C)** | Tool-agnostic, multi-platform | `{ "$value": "#3B82F6", "$type": "color" }` |
| **Tailwind config** | Tailwind CSS projects | `primary: '#3B82F6'` in `theme.extend.colors` |
| **Swift/Kotlin** | Native mobile | `static let primary = Color(hex: "#3B82F6")` |

---

## Common Mistakes

| Mistake | Why it's a problem | Fix |
|---------|-------------------|-----|
| **No semantic layer** | Can't theme or rebrand; `blue-500` hardcoded everywhere | Always have global → semantic → component tiers |
| **Naming drift** | `primary-blue`, `mainColor`, `brand-primary` in same system | Enforce naming convention from day one |
| **Over-tokenizing** | Token for every single pixel value, 500+ tokens nobody remembers | Tokenize decisions (semantic meaning), not every value |
| **Skipping component tier** | Components reference semantic tokens directly — works until you need component-specific overrides | Add component tokens for anything with variants or states |
| **No documentation** | Tokens exist in code but nobody knows what they're for | Every token needs a description and usage example |
| **Inconsistent scales** | Font sizes: 12, 14, 15, 17, 20 — no pattern | Use a mathematical scale (major second, minor third, etc.) |
| **Forgetting dark mode** | Tokens assume light background — dark mode is a retrofit nightmare | Design semantic tokens with modes in mind from the start |
