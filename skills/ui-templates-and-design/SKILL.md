# UI Templates, Visual Hierarchy & Web Design

> Distilled engineering knowledge for translating visual design into robust, reusable, and accessible web interfaces.

Design in software is not merely aesthetics; it is communication. A good template or component library codifies design decisions so engineers do not have to reinvent the wheel on every screen.

---

# Core Principles

Every UI system should satisfy:

**Consistency:** Users learn patterns. Do not break patterns unless the cost of learning the new pattern is outweighed by massive utility.
**Visual Hierarchy:** The eye should naturally flow to the most important element on the screen without conscious effort.
**Reusability:** Build templates and components that accept configuration, not hardcoded content.
**Responsiveness is Default:** Design systems must scale gracefully from mobile devices to ultra-wide monitors.

---

# Visual Hierarchy & Typography

Typography is 90% of web design.

**The Typographic Scale:** Do not use arbitrary font sizes. Establish a strict scale (e.g., modular scale based on a 1.125 ratio). Limit yourself to 5-6 sizes.
**Contrast & Legibility:** Text must meet WCAG AA standards for contrast (4.5:1 for normal text).
**Whitespace (Negative Space):** Whitespace is a structural element. Use it to group related items (Proximity Principle) and separate unrelated items. Avoid using borders when whitespace will suffice.
**Z-Pattern & F-Pattern:** Design layouts knowing that users scan screens in predictable patterns based on their reading language.

---

# Designing Reusable Templates

Templates are architectural blueprints for content.

**Slot-Based Architecture:** Design templates with "slots" (e.g., header, sidebar, main content, footer). Components injected into slots should not dictate the layout of the slot.
**Data-Driven Layouts:** Templates should adapt based on data availability. If a user has no data, show a well-designed Empty State, not a broken table.
**Themeability:** Templates should consume Design Tokens (CSS variables) for colors, spacing, and typography, allowing for effortless dark mode or white-labeling.

---

# Layout Patterns

Standardize how information is presented.

**Dashboards:** Prioritize scannability. Place critical KPIs at the top. Use grid systems (CSS Grid) to create modular, widget-based layouts.
**Forms:** Align labels above inputs for faster processing. Group related fields. Provide immediate, inline validation.
**Cards:** Use cards to group heterogeneous information about a single entity. Ensure the entire card is clickable if it represents a single link.

---

# Color Theory for Software

Color is functional.

**Primary/Brand:** Used for primary actions (Save, Submit).
**Semantic Colors:** Red for destructive/error, Green for success, Yellow for warning, Blue for information. Never rely on color alone to convey meaning (accessibility).
**Neutrals:** The most important palette. You need 8-10 shades of gray (from almost white to almost black) for backgrounds, borders, and text hierarchy.

---

# The ForgeCore UI Standard

A production-ready UI template is:
- **Accessible:** It passes automated screen reader and contrast tests by default.
- **Fluid:** It does not break when translated into German (long words) or viewed on an iPhone SE.
- **Systematic:** It is built from documented design tokens, not magic hex codes.
