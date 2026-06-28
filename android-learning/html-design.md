\# Lesson HTML Design Reference



Lessons must be \*\*beautiful, readable, and printable\*\*. A lesson the learner

returns to six months later should still feel like a quality resource.



\---



\## Typography



\- \*\*Body:\*\* `system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`

\- \*\*Code:\*\* `'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace`

\- \*\*Line height:\*\* 1.7–1.8 for body text; comfortable measure (60–70 chars)

\- \*\*Hierarchy:\*\* through size and weight, not decoration (no underlines,

&#x20; no all-caps headings)



\---



\## Code Blocks



\- Syntax highlighting via \[Highlight.js](https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js) from CDN

\- Language: `kotlin` for all Kotlin snippets, `xml` for layout files,

&#x20; `groovy`/`kotlin` for Gradle

\- Line numbers for any snippet longer than 5 lines

\- A subtle copy-to-clipboard button on each block



\---



\## Layout



\- Sticky top navigation bar with anchor links to each lesson section

\- Left-side annotation margin for callout notes (Tufte-style) on wider screens

\- Responsive: readable on a phone, printable on A4/letter

\- `@media print` hides: nav bar, interactive quiz buttons, solution toggles —

&#x20; leaves clean printable content



\---



\## Colour System



Use CSS custom properties for all colours so the lesson is adaptable:



```css

:root {

&#x20; --color-bg: #ffffff;

&#x20; --color-text: #1a1a1a;

&#x20; --color-accent: #4285F4;      /\* Android blue — use for Jetpack / Android topics \*/

&#x20; --color-accent-alt: #7F52FF;  /\* Kotlin purple — use for pure Kotlin topics \*/

&#x20; --color-code-bg: #f6f8fa;

&#x20; --color-border: #e1e4e8;

&#x20; --color-correct: #28a745;

&#x20; --color-wrong: #dc3545;

}



@media (prefers-color-scheme: dark) {

&#x20; :root {

&#x20;   --color-bg: #0d1117;

&#x20;   --color-text: #e6edf3;

&#x20;   --color-code-bg: #161b22;

&#x20;   --color-border: #30363d;

&#x20; }

}

```



Choose accent colour contextually: Android blue for Jetpack/Android APIs,

Kotlin purple for pure language features.



\---



\## Exercise Sections



\- Visually distinct from content: light background (`--color-code-bg`),

&#x20; rounded card (`border-radius: 8px`), subtle border

\- \*\*Quiz answers:\*\* All options styled identically before selection; correct /

&#x20; wrong state applied via JS class toggle — never make the correct answer

&#x20; visually distinct before the learner clicks

\- \*\*Code challenge stubs:\*\* Rendered in a standard code block; solution hidden

&#x20; in a `<details>` / `<summary>` toggle

\- \*\*Trace \& Prove expected outputs:\*\* Hidden in a `<details>` toggle — the

&#x20; learner forms a hypothesis \*before\* revealing the answer



\---



\## Callout Boxes



Use these consistently across lessons:



```html

<!-- Info / tip -->

<div class="callout callout-info">💡 Tip: ...</div>



<!-- Warning / pitfall -->

<div class="callout callout-warn">⚠️ Common mistake: ...</div>



<!-- Read-reference notice (Tier 3 top) -->

<div class="callout callout-read">

&#x20; 📖 Read this to understand where each piece lives.

&#x20; You don't need to run it now.

</div>

```



\---



\## Full Page Template Checklist



\- \[ ] `<meta charset="UTF-8">` and `<meta name="viewport" ...>` present

\- \[ ] Highlight.js CDN included and `hljs.highlightAll()` called in `<script>`

\- \[ ] All CSS in a single `<style>` block — no external stylesheets except CDN

\- \[ ] Sticky nav built with anchor links to every section heading

\- \[ ] `@media print` rule hides interactive elements

\- \[ ] Dark mode via `@media (prefers-color-scheme: dark)`

\- \[ ] All quiz JS is inline — no external dependencies beyond Highlight.js

\- \[ ] Solution toggles use native `<details>` / `<summary>` — no JS needed

