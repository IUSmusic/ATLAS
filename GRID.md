# ATLAS Grid System

ATLAS uses a first-class grid system for visual editing, source mapping, responsive layout, accessibility, and AI-agent reasoning.

---

## Default Grid

```text
base unit: 8 px
fine unit: 4 px
minimum interactive target: 44 x 44 px
snap: enabled
common spacing: 4, 8, 16, 24, 32, 40, 48, 64
```

---

## Responsibilities

The grid system controls:

- Visual overlay
- Drag snapping
- Resize snapping
- Alignment guides
- Column layout
- Baseline layout
- Safe areas
- Breakpoints
- Spacing tokens
- Layout validation
- Accessibility target validation
- AI layout context
- Source patch generation

---

## Overlay Modes

ATLAS should support:

- Base grid
- Fine grid
- Column grid
- Baseline grid
- Container bounds
- Safe area guides
- Alignment guides
- Spacing measurements
- Breakpoint preview
- Accessibility warnings

---

## Grid Event Model

```json
{
  "type": "grid.resize",
  "target": "PrimaryActionButton",
  "from": { "xUnits": 40, "yUnits": 23, "widthUnits": 16, "heightUnits": 5 },
  "to": { "xUnits": 40, "yUnits": 23, "widthUnits": 21, "heightUnits": 6 },
  "unit": 8,
  "snapped": true
}
```

---

## Source Patch Preference

ATLAS should prefer patches in this order:

1. Semantic layout change
2. Constraint or anchor change
3. Row or column placement
4. Tokenized grid-unit change
5. Raw pixel coordinate change

Raw pixel changes are a fallback, not the preferred representation.

---

## Accessibility Rules

ATLAS should flag:

- Interactive targets below 44 x 44 px
- Overlapping controls
- Poor spacing
- Missing accessible names
- Low contrast where measurable
- Keyboard navigation gaps
- Unclear focus order

---

## AI Integration

The grid model is included in agent context. The UI Agent must understand grid unit, snap status, container bounds, alignment intent, responsive breakpoint, minimum target size, and design tokens.
