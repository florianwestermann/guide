---
name: accessibility
description: WCAG 2.1 AA accessibility guidelines for UI design and engineering. Apply to every project, every component, every screen. Covers contrast, alt text, keyboard navigation, focus, and more.
---

# Accessibility Guidelines (WCAG 2.1 AA)

These rules are non-negotiable. Apply them to every interface, regardless of client.

---

## Colour & Contrast

- **Text contrast minimum: 4.5:1** against its background (normal text)
- **Large text (18px+ regular, 14px+ bold): 3:1** minimum
- **UI components and icons: 3:1** against adjacent colours (borders, icons, input outlines)
- Never convey information through colour alone — always pair with a label, icon, or pattern
- Test with: [Colour Contrast Analyser](https://www.tpgi.com/color-contrast-checker/) or [Stark](https://www.getstark.co/)

---

## Images & Alt Text

### Informative images
Describe what the image communicates, not what it looks like.
```html
<!-- ✅ -->
<img src="card.png" alt="Visa card ending in 4242" />

<!-- ❌ -->
<img src="card.png" alt="card image" />
` ``

### Decorative images
If the image adds no information, use an empty alt so screen readers skip it.
` ``html
<img src="background-texture.png" alt="" role="presentation" />
` ``

### Automatic alt generation (AI-assisted)
When generating alt text programmatically via an LLM or vision API:

- Prompt: *"Describe this image in one concise sentence as if explaining it to someone who cannot see it. Focus on meaning and context, not visual style."*
- Cap output at ~125 characters
- Strip filler phrases like "an image of" or "a photo showing"
- Always allow a human to review and override generated alt text
- Flag images where context is ambiguous for UX review: `<!-- 🚩 ALT REVIEW NEEDED -->`

### Complex images (charts, diagrams)
Provide a short alt + a longer text alternative nearby or via `aria-describedby`:
` ``html
<img src="chart.png" alt="Bar chart: Q1–Q4 revenue 2024" aria-describedby="chart-desc" />
<p id="chart-desc">Q1: CHF 1.2M, Q2: CHF 1.4M, Q3: CHF 1.1M, Q4: CHF 1.8M. Q4 highest.</p>
` ``

---

## Keyboard Navigation

Every interactive element must be reachable and operable with keyboard alone.

### Core rules
- All actions possible with a mouse must be possible with a keyboard
- Logical tab order follows visual reading order (top → bottom, left → right)
- Never remove focus outlines without replacing them with a visible custom style
- Trap focus inside modals and dialogs — return focus to the trigger on close

### Tab order
` ``html
<!-- ✅ Logical order -->
<input tabindex="0" />  <!-- default, follows DOM order -->

<!-- ❌ Never use positive tabindex — it breaks natural flow -->
<input tabindex="3" />
` ``

### Focus styles
Minimum visible focus indicator:
- 2px solid outline
- 3:1 contrast ratio between focus colour and background
` ``css
/* ✅ */
:focus-visible {
  outline: 2px solid #0055CC;
  outline-offset: 2px;
}

/* ❌ */
:focus { outline: none; }
` ``

### Interactive components
| Component | Keyboard behaviour |
|---|---|
| Button | `Enter` or `Space` to activate |
| Link | `Enter` to follow |
| Dropdown / Select | `Arrow keys` to navigate, `Enter` to select, `Esc` to close |
| Modal | `Esc` to close, focus trapped inside |
| Tabs | `Arrow keys` to switch tabs |
| Checkbox | `Space` to toggle |
| Date picker | `Arrow keys` for navigation, `Enter` to select |

---

## Semantic HTML

Use the right element for the right job — this is the fastest accessibility win.

` ``html
<!-- ✅ -->
<button>Submit</button>
<nav aria-label="Main navigation">...</nav>
<h1>Page title</h1>
<ul><li>Item</li></ul>

<!-- ❌ -->
<div onclick="submit()">Submit</div>
<div class="nav">...</div>
<p class="big-bold">Page title</p>
` ``

Heading hierarchy must be logical and never skip levels (h1 → h2 → h3).

---

## Forms

- Every input must have a visible, associated `<label>` (not just placeholder text)
- Placeholder disappears on input — never use it as the only label
- Group related fields with `<fieldset>` and `<legend>`
- Error messages must be:
  - Specific (*"Enter a valid Swiss IBAN (CH + 19 digits)"* not *"Invalid input"*)
  - Programmatically linked to the field via `aria-describedby`
  - Not colour-only — include an icon or text prefix

` ``html
<label for="iban">IBAN</label>
<input id="iban" aria-describedby="iban-error" aria-invalid="true" />
<span id="iban-error" role="alert">Enter a valid Swiss IBAN (e.g. CH56 0483 5012 3456 7800 9)</span>
` ``

---

## ARIA — use sparingly

Prefer native HTML. Only reach for ARIA when no native element exists.

` ``html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm deletion</h2>
</div>
` ``

| Attribute | When to use |
|---|---|
| `aria-label` / `aria-labelledby` | Names elements for screen readers |
| `aria-describedby` | Links additional description (errors, hints) |
| `aria-live="polite"` | Announces dynamic content updates (toasts, status) |
| `aria-hidden="true"` | Hides decorative elements from screen readers |

---

## Motion & Animation

- Respect `prefers-reduced-motion` — wrap all animations in a media query
- No content should flash more than 3 times per second (seizure threshold)

` ``css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
` ``

See `animations` skill for full motion guidelines.

---

## Testing Checklist

Before handing off any screen:

- [ ] Contrast checked (text + UI components)
- [ ] All images have meaningful alt text (or empty alt if decorative)
- [ ] Page navigable by keyboard only, tab order logical
- [ ] Focus indicator visible at all times
- [ ] All form fields labelled, errors descriptive
- [ ] Screen reader tested (VoiceOver / NVDA) on at least one key flow
- [ ] `prefers-reduced-motion` respected
- [ ] No information conveyed by colour alone
- [ ] Heading hierarchy is logical

---

## Tools

| Purpose | Tool |
|---|---|
| Contrast checking | [Colour Contrast Analyser](https://www.tpgi.com/color-contrast-checker/), Stark (Figma plugin) |
| Screen reader (macOS) | VoiceOver — `Cmd + F5` |
| Screen reader (Windows) | NVDA (free) |
| Automated audit | axe DevTools (browser extension) |
| Keyboard test | Unplug your mouse and try |
```

The backtick fences around code blocks have a stray space so they render here without breaking the outer block — remove those spaces when you paste (` `` ` → ```` ``` ````).