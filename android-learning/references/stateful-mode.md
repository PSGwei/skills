# Stateful Mode Files Reference

Use this file when creating a **wide-topic series** (multi-lesson). It
contains the exact format for every tracking file, with rules and examples.

Create these files **after** all lessons have been generated — not before.

---

## MISSION.md

Created once, at the start of a wide topic series. Captures the scope so
every lesson can be grounded in a concrete goal.

```md
# Mission: {Topic}

## Goal
{1–2 sentences: what the learner will be able to build or do in a real
Android project after mastering this topic.}

## Success looks like
- {A specific, observable thing — e.g. "Implement a ViewModel that survives
  rotation and exposes UI state via StateFlow"}
- {Another specific, observable thing}
- {…}

## Out of scope
- {Adjacent topics explicitly excluded — protects focus}
```

Rules:
- Be concrete. "Build a screen that loads data from a repository using
  coroutines" beats "understand coroutines."
- Keep it to one screen. If it runs longer, the scope is too broad.
- Update it if the learner's goal shifts mid-series. Confirm with the learner
  before changing.

---

## GLOSSARY.md

The canonical vocabulary for this series. Every term used across lessons
should live here once it has been taught.

```md
# {Topic} Glossary

{One or two sentences describing what this glossary covers.}

## Terms

**StateFlow**:
A hot Flow that holds a single value and emits it to all active collectors.
Thread-safe and lifecycle-independent — unlike LiveData, it does not require
a Lifecycle owner.
_Avoid_: observable state, reactive holder

**suspend fun**:
A Kotlin function that can pause execution without blocking its thread,
resuming automatically when an async result is ready.
_Avoid_: async function, coroutine function
```

Rules:
- Add a term **only after** it has been taught in a lesson — not as a preview
- Prefer official terminology from developer.android.com and kotlinlang.org
- Definitions: 1–2 sentences. What the term IS — not how to use it
- Cross-reference other glossary terms inside definitions where they apply
- Use `_Avoid_` to list informal, deprecated, or competing names
- Group under subheadings (`## Coroutines`, `## Compose`, `## Architecture`)
  when natural clusters emerge; a flat list is fine early on
- Revise definitions when later lessons deepen or correct an earlier
  understanding — do not leave stale entries

---

## RESOURCES.md

The curated set of trusted sources used across the series.

```md
# {Topic} Resources

## Official Documentation
- [Page title — developer.android.com](url)
  What it covers. When to reach for it during this series.

## Articles & Guides
- [Title — Author / Site](url)
  What it covers. When to reach for it.

## Gaps
- {An area the series needs to cover but no good resource has been found yet}
```

Rules:
- Annotate every entry: one line saying what it covers and when to use it.
  A bare URL is useless in three months.
- Official sources first, community resources second
- High-trust only — primary sources, recognised experts, well-moderated
  communities. Five sharp sources beat thirty mediocre ones.
- List gaps in `## Gaps` rather than leaving them silent — gaps drive future
  searches
- Prune ruthlessly. Remove any resource that turned out to be wrong, shallow,
  or off-topic

---

## Learning Records

Saved to `./learning-records/NNNN-<dash-case-name>.md`. Written **after**
each lesson is generated. Scan the directory for the highest existing number
and increment by one.

```md
# {Short title of the concept taught}

{1–3 sentences: what was covered in the lesson, and why it matters for the
next lesson in the series.}
```

Optional additions (only when they add genuine value):
- **Misconceptions pre-empted:** wrong beliefs this lesson explicitly addressed
- **Unlocks:** what this lesson makes it possible to teach next
- **Status:** `active | superseded by LR-NNNN` — use when a later lesson
  corrects an earlier understanding

Rules:
- Write after the lesson, not before
- Record that the concept was taught and what it unlocks — not a content
  summary of the lesson
- If a later record contradicts an earlier one, mark the old record
  `Status: superseded by LR-NNNN` rather than deleting it

---

## NOTES.md

A scratchpad for anything that should influence future lessons in this series:

- User preferences stated during any session (coding style choices, preferred
  analogies, pacing feedback)
- Topics the user already knows well — skip or reference only
- Difficulty calibration notes
  *("this learner finds coroutine scoping intuitive but struggles with
  dispatcher choice")*

**Check this file at the start of each new session** before deciding what
to teach next.