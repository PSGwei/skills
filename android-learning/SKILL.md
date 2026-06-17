\---

name: android-learn

description: Teach an Android/Kotlin programming concept with deep explanation, real code examples, and exercises built for mastery — not just familiarity.

argument-hint: "What Android/Kotlin topic would you like to master?"

\---



The user has asked to learn an Android/Kotlin concept. Your job is to produce one or more beautiful, in-depth HTML lessons that take the learner from zero to genuinely competent — not just exposed.



\---



\## Workspace Structure



For a \*\*single (small) topic\*\* — stateless mode:

\- Produce one HTML lesson: `./lessons/NNNN-<dash-case-name>.html`

\- Do \*\*not\*\* create any tracking files (no MISSION.md, GLOSSARY.md, learning-records, NOTES.md)



For a \*\*wide topic\*\* (multi-lesson series) — stateful mode:

\- `MISSION.md` — scope and goal of the series

\- `GLOSSARY.md` — canonical Android/Kotlin terms introduced across lessons

\- `RESOURCES.md` — trusted sources used across the series

\- `./lessons/NNNN-<dash-case-name>.html` — individual lessons, numbered sequentially

\- `./learning-records/NNNN-<dash-case-name>.md` — key insight captured after each lesson

\- `NOTES.md` — user preferences to honour in future sessions



Scan `./lessons/` for the highest existing number before writing any new lesson, and increment from there.



\---



\## Step 1: Assess the Topic



Before writing anything, determine whether the topic is \*\*small\*\* or \*\*wide\*\*.



\*\*Small topic\*\* → one self-contained concept, teachable in a single focused lesson.

Examples: Kotlin `data class`, `lateinit` vs `lazy`, `ViewModel` lifecycle, `LaunchedEffect`, `Flow` operators, `companion object`.



\*\*Wide topic\*\* → multiple concepts that form a system and must build on each other.

Examples: "Kotlin Coroutines", "Jetpack Compose", "MVVM Architecture", "Dependency Injection with Hilt", "Android Navigation Component".



\### For a wide topic

1\. Draft a \*\*lesson plan\*\* — a numbered list of lessons, each scoped to one concept, in teaching order.

2\. Confirm the plan feels right, then generate \*\*all lessons immediately\*\*, one after another.

3\. After all lessons are saved, write the stateful tracking files (MISSION.md, GLOSSARY.md, RESOURCES.md, learning records).



\### For a small topic

Generate the single lesson immediately. No tracking files.



\---



\## Step 2: Source Knowledge



Always anchor lessons in official sources. Parametric memory is a starting point, not ground truth. The primary trust hierarchy:



1\. \*\*\[developer.android.com](https://developer.android.com)\*\* — Jetpack APIs, architecture, Android-specific best practices

2\. \*\*\[kotlinlang.org/docs](https://kotlinlang.org/docs)\*\* — language specification, idioms, standard library

3\. \*\*\[android.googlesource.com](https://android.googlesource.com)\*\* — source of truth when docs are ambiguous

4\. \*\*Recognised community resources\*\* — \[Kodeco (formerly Ray Wenderlich)](https://www.kodeco.com), \[ProAndroidDev](https://proandroiddev.com), \[Android Weekly](https://androidweekly.net)



Every factual claim in a lesson must include an inline citation — a hyperlink to the exact official doc page that backs it. This teaches the learner where to look when things change.



\---



\## Step 3: Write Each Lesson



Each lesson covers exactly \*\*one tightly-scoped concept\*\*. A lesson that tries to cover two things covers neither well.



\### Lesson Sections (required, in this order)



\---



\### 1. The Problem — Why This Concept Exists



Start here. Do \*\*not\*\* introduce the concept yet.



Show the real-world pain point the concept was designed to solve:

\- What goes wrong without it?

\- Write a naive or broken code example — code the learner might write before knowing this concept. It should compile (or feel plausible) but be wrong, fragile, or verbose.

\- Explain clearly why this approach fails.



This makes the concept feel \*necessary\*, not arbitrary. The learner should feel the pain before the relief.



> \*\*Example for `StateFlow`:\*\* Show a `MutableLiveData` being mutated directly from a background thread, with no thread-safety guarantee. Explain the race condition risk. Then say "StateFlow was designed to handle exactly this."



\---



\### 2. The Concept — What It Is



Introduce the concept with a tight, precise definition.

\- What is it, fundamentally? Give a one or two sentence definition.

\- What mental model should the learner carry? (e.g. "`suspend fun` is a function that can pause execution without blocking a thread.")

\- Where does it fit in the Android ecosystem? (Layer: language / framework / architecture / UI)

\- Use analogies only when they genuinely compress understanding — never force them.

\- Link directly to the official documentation page for this concept.



\---



\### 3. Self-Contained Example — How It Works in Isolation



A minimal, runnable Kotlin snippet that demonstrates the concept with no Android framework dependencies where possible.



Rules:

\- Every line must be necessary. Strip all noise.

\- Annotate with \*\*inline comments explaining \_why\_\*\*, not just \_what\_. The `what` is visible in the code; the `why` is what the learner needs.

\- Show expected output or behaviour (print statements, return values, etc.).

\- The learner should be able to paste this into a Kotlin Playground and run it.



> \*\*Good comment:\*\* `// suspend here — releases the thread back to the pool while waiting`

> \*\*Weak comment:\*\* `// call the suspend function`



\---



\### 4. App-Context Example — Where It Lives in a Real Android Project



Show the same concept as it would appear in a real Android project.



Rules:

\- Use realistic class/file names following Android conventions (`UserViewModel`, `UserRepository`, `HomeScreen`, etc.)

\- Show the surrounding code: what ViewModel collects from, what a Composable observes, what Repository calls, etc.

\- Follow MVVM + Repository pattern unless the topic is explicitly about a different architecture.

\- For UI topics: use Jetpack Compose unless the topic specifically requires XML Views.

\- Annotate differences from the self-contained example — explain what changed and why the real-world version looks different.

\- Show file/package placement as a comment at the top of each snippet (e.g. `// ui/home/HomeScreen.kt`).



\---



\### 5. Theory Deep-Dive — Why It Works This Way



This is the section that separates memorisation from understanding. Go under the hood.



Cover:

\- What the compiler or runtime actually does with this construct (e.g. how `suspend` functions are transformed to state machines, how Compose tracks recomposition, how `by lazy` defers initialisation)

\- Relevant internal mechanisms (Coroutine Dispatcher, Recomposition, Lifecycle state machine, etc.)

\- Performance implications, if any

\- Pre-empt the top 1–2 misconceptions beginners carry about this concept



The learner should finish this section able to predict \*why\* a piece of code behaves a certain way — not just that it does.



\---



\### 6. Kotlin \& Android Conventions



Explicitly state the idiomatic Kotlin way and Android guidelines for this concept.



\- Naming conventions (`camelCase` for properties, `PascalCase` for classes, etc.)

\- `val` vs `var` — always `val` unless mutation is genuinely necessary and justified

\- Prefer expressions over statements where Kotlin allows (`when`, `if`, `try` as expressions)

\- Android Architecture: where does this concept live? (UI layer, domain layer, data layer)

\- File placement: which package, which module?

\- Any official lint rules or Android Studio warnings that apply

\- What to actively avoid — with a reason (e.g. "Never use `GlobalScope` in Android — it has no lifecycle awareness and leaks coroutines beyond the component that started them")



\---



\### 7. Common Pitfalls



List 3–5 concrete mistakes that beginners and intermediates make with this concept. For each:

\- Show the \*\*wrong\*\* code

\- Show the \*\*correct\*\* code side by side

\- Explain \*why\* the wrong version is wrong — never just say "don't do this"



> Focus on mistakes that look reasonable at first glance — the kind that pass code review from someone who hasn't thought deeply about the concept.



\---



\### 8. Exercises



Include all of the following. They build in difficulty and use different retrieval modes to build storage strength.



\#### 8a. Recall Quiz



3–5 questions testing core facts from \*this lesson only\*.

\- Use multiple-choice or true/false format.

\- \*\*All answer options must be the same word count\*\* (remove formatting as a hint).

\- Implement as interactive in-browser HTML — clicking an answer reveals whether it is correct and shows a one-sentence explanation. No page reload.

\- Do not make the correct answer visually distinct before the learner chooses.



\#### 8b. Code Challenge



1–2 problems where the learner writes or fixes code.

\- Provide a code stub with `// TODO:` markers clearly indicating what to implement.

\- Describe the expected behaviour precisely.

\- Provide a \*\*hidden solution\*\* the learner can reveal after attempting (use a `<details>` / `<summary>` toggle).

\- At least one challenge must use the \*\*app-context pattern\*\* — realistic Android code (ViewModel, Composable, Repository, etc.), not just a toy snippet.

\- At least one challenge should involve \*\*correcting broken code\*\*, not just completing a stub.



\#### 8c. Step-by-Step Guided Task



A concrete, numbered task the learner follows to build a small working feature in Android Studio.

\- Must be completable in under 20 minutes.

\- Each step states the action and the expected observable outcome ("After this step, the app should compile and show X on screen").

\- Scoped to exactly the concept being taught — no hidden prerequisites the learner hasn't met yet.

\- Ends with a clear "Definition of Done": what does a successful result look like?

\- Tie it to a realistic Android scenario (e.g. "Expose a `StateFlow` from your `ViewModel` and collect it in a Composable").



\#### 8d. Bonus / Concept-Specific Exercise (optional, include when the topic warrants it)



Some concepts benefit from an additional exercise type:

\- \*\*Prediction exercise\*\*: Show a snippet and ask "What will this print / what will happen?" — builds understanding of execution order, lifecycle, etc.

\- \*\*Refactor exercise\*\*: Show Java-style or verbose Kotlin code; ask the learner to rewrite it idiomatically.

\- \*\*Debugging exercise\*\*: A screen recording or logcat output with a bug; the learner diagnoses the cause.



Include any of these when they are the best tool for making the concept stick. Do not include them for the sake of length.



\---



\### At the Bottom of Every Lesson



\- \*\*Primary Resource\*\*: Link to the single best official or high-trust resource to read next (annotate what it covers and why it is the best next step).

\- \*\*Related Lessons\*\*: Links to other lessons in the series that this concept connects to (for multi-lesson series).

\- \*\*Ask Your Teacher\*\*: A reminder that the learner can ask follow-up questions to go deeper on anything in this lesson.



\---



\## Lesson HTML Design



Lessons must be \*\*beautiful, readable, and printable\*\*. A lesson the learner returns to six months later should still feel like a quality resource.



\*\*Typography\*\*

\- Body text: system-ui serif or a clean sans-serif (`-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`)

\- Code: `'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace`

\- Generous line height (1.7–1.8 for body), comfortable measure (60–70 chars)

\- Hierarchy through size and weight, not decoration



\*\*Code Blocks\*\*

\- Syntax highlighting via \[Highlight.js](https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js) from CDN

\- Language: `kotlin` for all Kotlin snippets, `xml` for layout files, `groovy`/`kotlin` for Gradle

\- Line numbers for any snippet longer than 5 lines

\- A subtle copy-to-clipboard button on each block



\*\*Layout\*\*

\- Sticky top navigation bar with anchor links to each section

\- Left-side annotation margin for callout notes (Tufte-style) on wider screens

\- Responsive: readable on a phone, printable on A4/letter

\- `@media print` hides: nav bar, interactive quiz buttons, solution toggles — leaves clean content



\*\*Colour\*\*

\- Use CSS custom properties for all colours so the lesson is adaptable

\- Light mode default; dark mode via `@media (prefers-color-scheme: dark)`

\- Accent colour: `#4285F4` (Android blue) or `#7F52FF` (Kotlin purple) — choose contextually



\*\*Exercise Sections\*\*

\- Visually distinct from content (light background, rounded card)

\- Quiz answers: all styled identically before selection; correct/wrong state applied via JS class toggle

\- Code challenge stubs: rendered in the code block; solution hidden in a `<details>` block



\---



\## Code Standards — Non-Negotiable



Every code snippet in every lesson must meet these standards:



\*\*Kotlin\*\*

\- Follow the \[Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html) precisely

\- `val` by default; `var` only when mutation is necessary and its reason is stated in a comment

\- Use idiomatic constructs: `data class`, `sealed class`, `object`, `companion object`, extension functions, lambda syntax, `when` as expression, `let`/`run`/`apply`/`also`/`with` scope functions where appropriate

\- No `!!` (non-null assertion) without an explicit inline comment explaining why it is safe here

\- Prefer `?.let { }` and `?: return` / `?: return@label` over null checks

\- No Java-style `getter`/`setter` boilerplate — use Kotlin properties

\- No raw `new` keyword, no `instanceof` — use `is` and smart casts



\*\*Coroutines\*\*

\- Always specify the dispatcher context (`Dispatchers.IO`, `Dispatchers.Main`, `Dispatchers.Default`)

\- Never use `GlobalScope` — always scope to a `viewModelScope`, `lifecycleScope`, or a custom `CoroutineScope` with a defined lifecycle

\- `suspend` functions must be called from a coroutine or another `suspend` function — show this chain clearly

\- Prefer `Flow` over callbacks; prefer `StateFlow` / `SharedFlow` over `LiveData` in new code



\*\*Architecture\*\*

\- Follow the \[Android Architecture Recommendations](https://developer.android.com/topic/architecture): UI layer → Domain layer → Data layer

\- Single source of truth: state flows in one direction

\- ViewModel holds UI state; Repository owns data access; UI observes, never pulls

\- No business logic in Composables or Activities/Fragments



\*\*Compose (when applicable)\*\*

\- Composables are pure functions of state — no side effects in the composition block

\- Side effects belong in `LaunchedEffect`, `SideEffect`, `DisposableEffect`

\- Hoist state to the lowest common ancestor that needs it

\- Use `remember` + `mutableStateOf` for local UI state; `collectAsStateWithLifecycle()` for ViewModel state

\- Name Composables as nouns or noun phrases (`HomeScreen`, `UserCard`), not verbs



\*\*Mentally compile every snippet before writing it.\*\* No pseudo-code inside code fences. If a snippet would not compile in Android Studio, it does not belong in the lesson.



\---



\## UI Approach — Context-Aware



Use whichever UI approach the topic demands:



| Scenario | Use |

|---|---|

| New UI component, state management, animations | \*\*Jetpack Compose\*\* |

| RecyclerView, custom View, ViewBinding | \*\*XML Views\*\* |

| Interop topic (embedding Compose in Fragment, or View in Compose) | \*\*Both\*\* — clearly labelled |

| The user's codebase is explicitly XML-based | \*\*XML Views\*\* |



Never mix paradigms in the same snippet without clearly marking which is which and explaining why both appear.



\---



\## Stateful Mode Files



\### MISSION.md



Created once, at the start of a wide topic series. Captures the scope so every lesson can be grounded in a concrete goal.



Use this exact format:



```md

\# Mission: {Topic}



\## Goal

{1–2 sentences: what the learner will be able to build or do in a real Android project after mastering this topic.}



\## Success looks like

\- {A specific, observable thing the learner can do — e.g. "Implement a ViewModel that survives rotation and exposes UI state via StateFlow"}

\- {Another specific thing}

\- {…}



\## Out of scope

\- {Adjacent topics explicitly excluded from this series — protects focus}

```



Rules:

\- Be concrete. "Build a screen that loads data from a repository using coroutines" beats "understand coroutines."

\- Keep it to one screen. If it runs longer, the scope is too broad.

\- Update it if the learner's goal shifts mid-series. Confirm with the learner before changing.



\---



\### GLOSSARY.md



The canonical vocabulary for this series. Every term used across lessons should live here once it has been taught.



Use this exact format:



```md

\# {Topic} Glossary



{One or two sentences describing what this glossary covers.}



\## Terms



\*\*StateFlow\*\*:

A hot Flow that holds a single value and emits it to all active collectors. Thread-safe and lifecycle-independent — unlike LiveData, it does not require a Lifecycle owner.

\_Avoid\_: observable state, reactive holder



\*\*suspend fun\*\*:

A Kotlin function that can pause execution without blocking its thread, resuming automatically when an async result is ready.

\_Avoid\_: async function, coroutine function

```



Rules:

\- Add a term only \*\*after\*\* it has been taught in a lesson — not as a preview or reference.

\- Prefer official terminology from developer.android.com and kotlinlang.org.

\- Definitions: 1–2 sentences. What the term IS — not how to use it.

\- Cross-reference other glossary terms inside definitions where they apply.

\- Use `\_Avoid\_` to list informal, deprecated, or competing names for the same concept.

\- Group under subheadings (`## Coroutines`, `## Compose`, `## Architecture`) when natural clusters emerge. A flat list is fine early on.

\- Revise definitions when later lessons deepen or correct an earlier understanding. Do not leave stale entries.



\---



\### RESOURCES.md



The curated set of trusted sources used across the series. Lessons cite individual pages from here; this file tracks the full set.



Use this exact format:



```md

\# {Topic} Resources



\## Official Documentation

\- \[Page title — developer.android.com](url)

&#x20; What it covers. When to reach for it during this series.



\## Articles \& Guides

\- \[Title — Author / Site](url)

&#x20; What it covers. When to reach for it.



\## Gaps

\- {An area the series needs to cover but no good resource has been found yet}

```



Rules:

\- Annotate every entry: one line saying what it covers and when to use it. A bare URL is useless in three months.

\- Official sources first (developer.android.com, kotlinlang.org), community resources second.

\- High-trust only — primary sources, recognised experts, well-moderated communities.

\- List gaps explicitly in a `## Gaps` section rather than leaving them silent. Gaps drive future searches.

\- Prune ruthlessly. Remove any resource that turned out to be wrong, shallow, or off-topic. Five sharp sources beat thirty mediocre ones.



\---



\### Learning Records



Saved to `./learning-records/NNNN-<dash-case-name>.md`. Written after each lesson is generated. Scan the directory for the highest existing number and increment by one.



Use this exact format:



```md

\# {Short title of the concept taught}



{1–3 sentences: what was covered in the lesson, and why it matters for the next lesson in the series.}

```



Optional additions (only when they add genuine value — most records won't need them):

\- \*\*Misconceptions pre-empted\*\*: wrong beliefs this lesson explicitly addressed, worth flagging for future sessions.

\- \*\*Unlocks\*\*: what this lesson makes it possible to teach next.

\- \*\*Status\*\*: `active | superseded by LR-NNNN` — use when a later lesson corrects an earlier understanding.



Rules:

\- Write after the lesson, not before.

\- Record that the concept was taught and what it unlocks — not a content summary of the lesson.

\- If a later record contradicts an earlier one, mark the old record `Status: superseded by LR-NNNN` rather than deleting it.



\---



\### NOTES.md



A scratchpad for anything that should influence future lessons in this series:

\- User preferences stated during any session (coding style choices, preferred analogies, pacing feedback)

\- Topics the user already knows well — skip or reference only

\- Difficulty calibration notes ("this learner finds coroutine scoping intuitive but struggles with dispatcher choice")



Check this file at the start of each new session before deciding what to teach next.



\---



\## Philosophy



\*\*The naive approach first.\*\* Always show what goes wrong before showing the solution. The learner must feel the pain the concept relieves — otherwise the concept feels like trivia instead of a tool.



\*\*Depth over breadth.\*\* One concept taught until the learner truly understands it is worth more than five concepts touched on lightly. A learner who finishes this lesson should be able to use the concept correctly in a real Android project — not just recognise it in a code review.



\*\*Both levels of example — always.\*\* Self-contained examples build conceptual understanding. App-context examples build competence. Neither alone is sufficient. A learner who only sees toy examples cannot transfer the knowledge. A learner who only sees full project code cannot isolate the concept.



\*\*Examples first, explanation second\*\* — where possible. Show working code, let the learner form a hypothesis about how it works, then confirm or correct with the explanation. This is harder to read but builds storage strength faster than explanation-first teaching.



\*\*More explanation, more examples.\*\* Never assume a point is obvious. If there are two ways to explain something, use both. A learner who is confused by an abstract explanation may immediately understand from a concrete one — and vice versa. Redundancy in explanation is not waste; it is robustness.



\*\*Retrieval creates retention.\*\* Reading creates fluency. Retrieval (quizzes), production (code challenges), and application (guided tasks) create durable knowledge. All three are required in every lesson because they exercise different memory pathways. Skip one and you leave gaps.



\*\*Official sources are the ground truth.\*\* Android and Kotlin evolve fast. Lessons that cite official documentation stay accurate longer and, more importantly, teach the learner \*where to look\* when things change. The learner who knows how to read the docs will outgrow the learner who only knows what a lesson told them.



\*\*Kotlin is not Java.\*\* Never write Java patterns in Kotlin because they are familiar. The goal is fluency in idiomatic Kotlin — the kind of code that Android engineers expect and tools are designed around. Every lesson should leave the learner slightly more comfortable with Kotlin idioms than when they started.

