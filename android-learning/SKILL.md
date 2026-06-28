\---

name: android-learn

description: >

&#x20; Teach an Android/Kotlin programming concept with deep explanation, real code

&#x20; examples, and exercises built for mastery — not just familiarity.

&#x20; Use this skill whenever the user wants to learn, understand, or explore any

&#x20; Android or Kotlin topic. Trigger on phrases like "explain X to me",

&#x20; "how does X work", "teach me X", "I don't understand X", "walk me through X",

&#x20; "what is X in Android/Kotlin", or "show me an example of X".

&#x20; Also trigger for questions about: ViewModel, Coroutines, Flow, StateFlow,

&#x20; SharedFlow, LiveData, Jetpack Compose, Hilt, MVVM, Clean Architecture,

&#x20; Navigation Component, Room, Retrofit, WorkManager, data class, sealed class,

&#x20; companion object, lateinit, lazy, suspend functions, Dispatchers, lifecycle,

&#x20; Composable, LaunchedEffect, remember, recomposition, or any other

&#x20; Android/Kotlin concept. Trigger even when the user wants to compare two

&#x20; concepts, debug their understanding, or deepen partial knowledge.

\---



\## Table of Contents



| # | Section | Reference file |

|---|---------|----------------|

| 1 | \[Workspace Structure](#workspace) | — |

| 2 | \[Step 1: Assess the Topic](#assess) | — |

| 3 | \[Step 2: Source Knowledge](#source) | — |

| 4 | \[Step 3: Write Each Lesson](#write) | `references/lesson-structure.md` |

| 5 | \[Step 4: Code Self-Review](#review) | `references/code-standards.md` |

| 6 | \[Lesson HTML Design](#html) | `references/html-design.md` |

| 7 | \[UI Approach](#ui) | — |

| 8 | \[Stateful Mode Files](#stateful) | `references/stateful-mode.md` |

| 9 | \[Philosophy](#philosophy) | — |



\---



\## Workspace Structure {#workspace}



\*\*Stateless mode\*\* (small / single topic):

\- One HTML lesson: `./lessons/NNNN-<dash-case-name>.html`

\- No tracking files — no MISSION.md, GLOSSARY.md, learning-records, NOTES.md



\*\*Stateful mode\*\* (wide / multi-lesson series):

```

MISSION.md               ← scope and goal of the series

GLOSSARY.md              ← canonical terms introduced across lessons

RESOURCES.md             ← trusted sources used across the series

lessons/

&#x20; NNNN-<name>.html       ← individual lessons, numbered sequentially

learning-records/

&#x20; NNNN-<name>.md         ← key insight captured after each lesson

NOTES.md                 ← user preferences, pacing notes, calibration

```



Scan `./lessons/` for the highest existing number before writing any new lesson,

and increment from there.



\---



\## Step 1: Assess the Topic {#assess}



Determine \*\*small\*\* or \*\*wide\*\* before writing anything.



\*\*Small topic\*\* → one self-contained concept, one lesson.

\*Examples\*: `data class`, `lateinit` vs `lazy`, `ViewModel` lifecycle,

`LaunchedEffect`, `Flow` operators, `companion object`.



\*\*Wide topic\*\* → multiple interdependent concepts that build on each other.

\*Examples\*: Kotlin Coroutines, Jetpack Compose, MVVM Architecture,

Dependency Injection with Hilt, Android Navigation Component.



\### Wide topic workflow

1\. Draft a \*\*lesson plan\*\* — numbered lessons in teaching order, one concept each.

2\. Share the plan with the learner; confirm before proceeding.

3\. Generate \*\*all lessons immediately\*\*, one after another.

4\. After all lessons are saved, create the stateful tracking files.



\### Small topic workflow

Generate the single lesson immediately. No tracking files.



\---



\## Step 2: Source Knowledge {#source}



Parametric memory is a starting point, not ground truth. Always anchor in

official sources.



\*\*Trust hierarchy:\*\*

1\. \*\*\[developer.android.com](https://developer.android.com)\*\* — Jetpack APIs, architecture, Android best practices

2\. \*\*\[kotlinlang.org/docs](https://kotlinlang.org/docs)\*\* — language spec, idioms, stdlib

3\. \*\*\[android.googlesource.com](https://android.googlesource.com)\*\* — source of truth when docs are ambiguous

4\. \*\*Community\*\* — \[Kodeco](https://www.kodeco.com), \[ProAndroidDev](https://proandroiddev.com), \[Android Weekly](https://androidweekly.net)



Every factual claim in a lesson must include an inline citation — a hyperlink

to the exact official doc page that backs it. This teaches the learner where

to look when things change.



\---



\## Step 3: Write Each Lesson {#write}



> 📖 \*\*Read `references/lesson-structure.md` before writing any lesson.\*\*

> Contains the full spec for all 7 required sections, the three-tier example

> system (Playground → Single-File Realistic → Production Reference), all

> exercise types, and what appears at the bottom of every lesson.



\*\*Core rule:\*\* Each lesson covers exactly \*\*one tightly-scoped concept\*\*.

A lesson that tries to cover two things covers neither well.



\---



\## Step 4: Code Self-Review {#review}



> 📖 \*\*Read `references/code-standards.md` before finalising any code.\*\*

> Contains all non-negotiable code standards (Imports, Kotlin idioms,

> Coroutines, Architecture, Compose) merged with the full self-review

> checklists. Run every checklist item on every snippet before generating HTML.



This step is \*\*mandatory\*\* — run it after writing all snippets, before

touching the HTML template.



\---



\## Lesson HTML Design {#html}



> 📖 \*\*Read `references/html-design.md` for the full rendering spec.\*\*

> Covers: typography, code block syntax highlighting via Highlight.js, sticky

> nav layout, responsive + print styles, colour system, and exercise UI

> (quiz cards, solution toggles, trace-and-prove sections).



\---



\## UI Approach — Context-Aware {#ui}



| Scenario | Use |

|---|---|

| New UI component, state management, animations | \*\*Jetpack Compose\*\* |

| RecyclerView, custom View, ViewBinding | \*\*XML Views\*\* |

| Interop (Compose in Fragment, or View in Compose) | \*\*Both\*\* — clearly labelled |

| User's codebase is explicitly XML-based | \*\*XML Views\*\* |



Never mix paradigms in the same snippet without clearly labelling which is

which and explaining why both appear.



\---



\## Stateful Mode Files {#stateful}



> 📖 \*\*Read `references/stateful-mode.md` when creating a wide-topic series.\*\*

> Contains the exact formats for MISSION.md, GLOSSARY.md, RESOURCES.md,

> Learning Records, and NOTES.md — including rules and examples for each.



\---



\## Philosophy {#philosophy}



\*\*The naive approach first.\*\* Always show what goes wrong before showing the

solution. The learner must feel the pain the concept relieves — otherwise it

feels like trivia instead of a tool.



\*\*Depth over breadth.\*\* One concept taught until the learner truly understands

it is worth more than five concepts touched on lightly. The learner who

finishes this lesson should be able to use the concept correctly in a real

Android project — not just recognise it in a code review.



\*\*Three tiers of example — always.\*\* Tier 1 (Playground) proves the concept

in 60 seconds with zero setup. Tier 2 (Single-File Realistic) builds pattern

competence without architecture overhead. Tier 3 (Production Reference) shows

where each piece lives in a real codebase. All three are required every time.



\*\*Examples first, explanation second.\*\* Show working code, let the learner

form a hypothesis, then confirm or correct with the explanation. This builds

storage strength faster than explanation-first teaching.



\*\*More explanation, more examples.\*\* Never assume a point is obvious.

Redundancy in explanation is not waste — it is robustness.



\*\*Retrieval creates retention.\*\* Reading creates fluency. Quizzes, code

challenges, and guided tasks create durable knowledge. All three are required

because they exercise different memory pathways.



\*\*Official sources are the ground truth.\*\* Android and Kotlin evolve fast.

Lessons that cite official docs stay accurate longer and teach the learner

where to look when things change.



\*\*Kotlin is not Java.\*\* Never write Java patterns in Kotlin because they are

familiar. The goal is idiomatic Kotlin — the kind of code Android engineers

expect and tools are designed around.

