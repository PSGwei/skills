\# Lesson Structure Reference



Each lesson covers exactly \*\*one tightly-scoped concept\*\* and must include

all 7 sections below, in order.



\---



\## 1. The Problem — Why This Concept Exists



Start here. Do \*\*not\*\* introduce the concept yet.



Show the real-world pain point the concept was designed to solve:

\- What goes wrong without it?

\- Write a naive or broken code example — code the learner might write before

&#x20; knowing this concept. It should compile (or feel plausible) but be wrong,

&#x20; fragile, or verbose.

\- Explain clearly why this approach fails.



This makes the concept feel \*necessary\*, not arbitrary. The learner should

feel the pain before the relief.



> \*\*Example for `StateFlow`:\*\* Show a `MutableLiveData` being mutated directly

> from a background thread with no thread-safety guarantee. Explain the race

> condition risk. Then say "StateFlow was designed to handle exactly this."



\---



\## 2. The Concept — What It Is



Introduce the concept with a tight, precise definition:

\- What is it, fundamentally? (1–2 sentence definition)

\- What mental model should the learner carry?

&#x20; \*(e.g. "`suspend fun` is a function that can pause execution without

&#x20; blocking a thread.")\*

\- Where does it fit in the Android ecosystem?

&#x20; (layer: language / framework / architecture / UI)

\- Use analogies only when they genuinely compress understanding — never force them.

\- Link directly to the official documentation page for this concept.



\---



\## 3. Examples — Three Tiers



Every lesson must include \*\*all three tiers\*\*. They serve different learner

states and must never be skipped.



| Tier | Name | Setup required | Purpose |

|---|---|---|---|

| 1 | \*\*Playground\*\* | Zero — paste into `setContent { }` | Prove the concept works in 60 seconds |

| 2 | \*\*Single-File Realistic\*\* | One Kotlin file — no Hilt, no Repository | Practice the pattern without architecture overhead |

| 3 | \*\*Production Reference\*\* | Full MVVM across multiple files | Show where each piece lives in a real codebase |



\### Rules common to all tiers



\- \*\*Complete explicit imports:\*\* Every snippet must begin with a full import

&#x20; block. No wildcard imports (`import androidx.compose.material3.\*`) — use

&#x20; explicit imports so the learner sees exactly what is being pulled in.

&#x20; Group order: Android SDK / framework → Jetpack / Compose / AndroidX →

&#x20; Kotlin stdlib → third-party.

\- \*\*Comments explain \*why\*, not \*what\*:\*\* Inline comments must explain the

&#x20; reasoning behind a line — the `what` is already visible in the code itself.

&#x20; - ✅ Good: `// remember keeps the same State instance across recompositions — without this, counter resets on every recompose`

&#x20; - ❌ Weak: `// remember the state`

\- \*\*Tier header label:\*\* Every snippet must carry its tier label as its very

&#x20; first comment line.



\### Tier 1 — Playground



A minimal snippet the learner can paste into `setContent { }` inside any

existing Activity and run immediately.



Rules:

\- Zero external dependencies beyond a blank Compose project

\- No ViewModel, no Hilt, no Repository — the concept in its purest form

\- Every line must be necessary; if it doesn't demonstrate the concept, remove it

\- Tier label: `// ✏️ Tier 1 — Paste inside setContent { }`

\- \*\*Immediately before the snippet\*\*, write a "What this snippet shows" note

&#x20; (1–2 sentences) based on the actual code — not a generic description.

&#x20; Name the specific classes, functions, or behaviors the learner will observe.



&#x20; ✅ Good: \*"This snippet shows how `mutableStateOf()` creates an observable

&#x20; value, and how reading it inside `Text()` makes Compose automatically

&#x20; re-render the label every time the button is clicked."\*

&#x20; ❌ Weak: \*"This is a simple example to get you started with state."\*



\### Tier 2 — Single-File Realistic



The same concept in a believable UI pattern, but with \*\*everything in one

Kotlin file\*\*.



Rules:

\- Use `viewModel()` (not Hilt injection) with a simple hand-written ViewModel

\- Use hardcoded / fake data inline (`listOf(...)`) — no Repository class

\- Use realistic class names: `MoviesViewModel`, `HomeScreen`, `TaskCard`

\- Structure the file top-to-bottom: Data Model → UI State sealed class →

&#x20; ViewModel → Composable(s)

\- Tier label: `// ✏️ Tier 2 — Single file, paste into your project and run`

\- \*\*Immediately before the snippet\*\*, write a "What this tier adds" note

&#x20; (1–2 sentences) explaining specifically how this extends Tier 1.



&#x20; ✅ Good: \*"Unlike Tier 1 where state lived in a `remember` inside the

&#x20; Composable, here `CounterViewModel` owns the `StateFlow` — so the count

&#x20; survives screen rotation and the Composable has no state logic of its own."\*

&#x20; ❌ Weak: \*"This adds a ViewModel to make the example more realistic."\*



\### Tier 3 — Production Reference



The full production pattern split across the files where it would live in a

real project. This is a \*\*reading reference\*\*, not a running exercise.



Rules:

\- Open each file snippet with its file path (`// ui/movies/MoviesViewModel.kt`),

&#x20; then its full import block immediately after

\- Use full MVVM + Repository with Hilt (or clearly commented manual DI if

&#x20; Hilt hasn't been taught yet)

\- Follow Android Architecture Recommendations: UI layer → Domain layer → Data layer

\- Add a callout box at the top of this tier:

&#x20; \*"📖 Read this to understand where each piece lives. You don't need to run it now."\*

\- Cross-reference Tier 2 explicitly:

&#x20; \*"The inline ViewModel in Tier 2 maps to `MoviesViewModel.kt` here"\*

\- Tier label: `// 📖 Tier 3 — Production layout (reference only)`

\- \*\*Before each file snippet\*\*, write a "Role of this file" line (1 sentence)

&#x20; naming the architectural responsibility and how it connects to the next file.



&#x20; ✅ Good: \*"`CounterViewModel.kt` — owns the `StateFlow<Int>` and exposes

&#x20; `increment()` to the UI layer; it has no knowledge of Compose."\*

&#x20; ❌ Weak: \*"This is the ViewModel file."\*



\---



\## 4. Theory Deep-Dive — Why It Works This Way



This section separates memorisation from understanding. Go under the hood.



Cover:

\- What the compiler or runtime actually does with this construct

&#x20; \*(e.g. how `suspend` functions are transformed to state machines, how

&#x20; Compose tracks recomposition, how `by lazy` defers initialisation)\*

\- Relevant internal mechanisms (Coroutine Dispatcher, Recomposition, Lifecycle

&#x20; state machine, etc.)

\- Performance implications, if any

\- Pre-empt the top 1–2 misconceptions beginners carry about this concept



The learner should finish this section able to \*\*predict\*\* \*why\* a piece of

code behaves a certain way — not just that it does.



\---



\## 5. Kotlin \& Android Conventions



Explicitly state the idiomatic Kotlin way and Android guidelines:



\- Naming conventions (`camelCase` for properties, `PascalCase` for classes)

\- `val` vs `var` — always `val` unless mutation is genuinely necessary

\- Prefer expressions over statements (`when`, `if`, `try` as expressions)

\- Android Architecture: which layer does this concept belong to?

\- File placement: which package, which module?

\- Any official lint rules or Android Studio warnings that apply

\- What to actively avoid — with a reason

&#x20; \*(e.g. "Never use `GlobalScope` in Android — it has no lifecycle awareness

&#x20; and leaks coroutines beyond the component that started them")\*



\---



\## 6. Common Pitfalls



List 3–5 concrete mistakes beginners and intermediates make. For each:

\- Show the \*\*wrong\*\* code

\- Show the \*\*correct\*\* code side by side

\- Explain \*why\* the wrong version is wrong — never just say "don't do this"



> Focus on mistakes that look reasonable at first glance — the kind that pass

> code review from someone who hasn't thought deeply about the concept.



\---



\## 7. Exercises



Include all four exercise types below. They build in difficulty and use

different retrieval modes to build storage strength.



\### 7a. Recall Quiz



3–5 questions testing core facts from \*this lesson only\*.

\- Multiple-choice or true/false format

\- \*\*All answer options must be the same word count\*\* (removes formatting as a hint)

\- Interactive in-browser HTML — clicking reveals whether correct + a one-sentence

&#x20; explanation. No page reload.

\- Do not make the correct answer visually distinct before the learner chooses



\### 7b. Code Challenge



1–2 problems where the learner writes or fixes code:

\- Provide a code stub with `// TODO:` markers clearly indicating what to implement

\- Describe the expected behaviour precisely

\- Provide a \*\*hidden solution\*\* the learner reveals after attempting

&#x20; (use a `<details>` / `<summary>` toggle)

\- At least one challenge must use the \*\*Tier 2 or Tier 3 pattern\*\* — realistic

&#x20; Android code (ViewModel, Composable, fake/real data), not just a toy snippet

\- At least one challenge should involve \*\*correcting broken code\*\*, not just

&#x20; completing a stub



\### 7c. Step-by-Step Guided Task



A concrete, numbered task the learner follows in Android Studio:

\- Completable in under 20 minutes

\- Each step states the action \*\*and\*\* the expected observable outcome

&#x20; \*("After this step, the app should compile and show X on screen")\*

\- Scoped to exactly the concept being taught — no hidden prerequisites

\- Ends with a clear "Definition of Done"

\- Tied to a realistic Android scenario



\### 7d. Trace \& Prove + Concept-Specific Exercise



\#### Trace \& Prove (always required)



The learner instruments real Android code with `Log` statements to

\*empirically verify\* a claim from this lesson — moving from "I read it"

to "I saw it myself."



Structure every Trace \& Prove in this exact order:



\*\*1. The Claim\*\*

State the theory as a single bold assertion.

> \*Example: "A `StateFlow` collector always receives the current value

> immediately upon subscription, even if emitted before the collector started."\*



\*\*2. The Setup\*\*

Minimal runnable code stub with `// 🔍 ADD LOG HERE` markers.

\- Tag convention: `"TraceProve"` — consistent across all lessons

\- Logcat filter: `tag:TraceProve`

\- Under 30 lines; every log line carries thread name, value, and a short label:

&#x20; ```kotlin

&#x20; Log.d("TraceProve", "thread=${Thread.currentThread().name} | value=$value")

&#x20; ```

\- For pure Kotlin topics: `println()` in Kotlin Playground is acceptable;

&#x20; clearly note this in the exercise



\*\*3. Scenario A — Run \& Observe\*\*

\- State the action: \*"Launch the app and navigate to HomeScreen"\*

\- Show the \*\*expected Logcat output\*\* inside a `<details>` toggle

\- Ask: \*"Does this match what you expected? Why does the output look this way?"\*



\*\*4. Scenario B — The Contrasting Case\*\*

\- Change one condition to surface a nuance or edge case

\- Show expected output in a `<details>` toggle

\- Ask: \*"What changed? What does that tell you about the concept?"\*



\*\*5. Explain It\*\*

A written reflection prompt the learner answers in their own words.



Rules:

\- The Claim must be directly verifiable from Logcat — no indirect inference

\- Always include one Scenario that challenges an assumption formed from the lesson

\- Never use `System.out.println()` in Android context — always `Log.d()` with `"TraceProve"` tag

\- Hide expected output in a `<details>` toggle so the learner predicts before revealing



\#### Concept-Specific Exercise (optional)



Include any of these when they're the best tool for making the concept stick:

\- \*\*Prediction exercise\*\*: "What will this print / what will happen?"

\- \*\*Refactor exercise\*\*: Rewrite Java-style or verbose Kotlin code idiomatically

\- \*\*Debugging exercise\*\*: Diagnose and fix a broken snippet or Logcat output



Do not include for the sake of length.



\---



\## At the Bottom of Every Lesson



\- \*\*Primary Resource:\*\* Link to the single best official or high-trust resource

&#x20; to read next (annotate what it covers and why it is the best next step)

\- \*\*Related Lessons:\*\* Links to other lessons in the series (multi-lesson

&#x20; series only)

\- \*\*Ask Your Teacher:\*\* A reminder that the learner can ask follow-up questions

&#x20; to go deeper on anything in this lesson

