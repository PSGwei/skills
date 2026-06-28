# Code Standards & Self-Review Reference

Everything in this file is **non-negotiable**. Apply all standards to every
snippet in every lesson — Tier 1, Tier 2, Tier 3, Trace & Prove, Code
Challenges — before generating any HTML.

> **Why this step exists:** Code generated in a single flow often contains
> subtle errors — a missing import, an unbalanced brace, a `@Composable`
> called from a non-composable context. This review catches those before the
> learner encounters them.

---

## Code Standards

### Imports

- Every snippet must begin with a **complete, explicit import block** — no
  wildcard imports (`import androidx.compose.material3.*`), no missing imports,
  no inherited-from-context assumptions
- Group imports in this order:
  Android SDK / framework → Jetpack / Compose / AndroidX → Kotlin stdlib → third-party
- Every class, function, annotation, and extension used in the snippet must
  have a matching import
- A snippet without complete imports is **incomplete** — treat it as a build error

### Kotlin Idioms

Follow the [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html) precisely:
- `val` by default; `var` only when mutation is necessary — and its reason is
  stated in a comment
- Use idiomatic constructs: `data class`, `sealed class`, `object`,
  `companion object`, extension functions, lambda syntax, `when` as
  expression, scope functions (`let`, `run`, `apply`, `also`, `with`) where
  appropriate
- No `!!` without an explicit inline comment explaining why it is safe here
- Prefer `?.let { }` and `?: return` / `?: return@label` over null checks
- No Java-style `getter`/`setter` boilerplate — use Kotlin properties
- No raw `new` keyword, no `instanceof` — use `is` and smart casts

### Coroutines

- Always specify the dispatcher context (`Dispatchers.IO`, `Dispatchers.Main`,
  `Dispatchers.Default`)
- **Never use `GlobalScope`** — always scope to `viewModelScope`,
  `lifecycleScope`, or a custom `CoroutineScope` with a defined lifecycle
- `suspend` functions must be called from a coroutine or another `suspend`
  function — show this call chain clearly in the snippet
- Prefer `Flow` over callbacks; prefer `StateFlow` / `SharedFlow` over
  `LiveData` in new code

### Architecture

Follow the [Android Architecture Recommendations](https://developer.android.com/topic/architecture):
- **UI layer → Domain layer → Data layer**
- Single source of truth: state flows in one direction
- ViewModel holds UI state; Repository owns data access; UI observes, never pulls
- **No business logic in Composables or Activities/Fragments**

### Compose (when applicable)

- Composables are pure functions of state — **no side effects in the
  composition block**
- Side effects belong in `LaunchedEffect`, `SideEffect`, or `DisposableEffect`
- Hoist state to the lowest common ancestor that needs it
- Use `remember` + `mutableStateOf` for local UI state;
  `collectAsStateWithLifecycle()` for ViewModel state
- Name Composables as nouns or noun phrases (`HomeScreen`, `UserCard`),
  not verbs

**Mentally compile every snippet before writing it.** No pseudo-code inside
code fences. If a snippet would not compile in Android Studio, it does not
belong in the lesson.

---

## Self-Review Checklists

Run every item below on **every snippet** in the lesson. Fix any issue before
proceeding to HTML generation.

### ✅ Imports Checklist

- [ ] A complete import block is present at the top of the snippet
- [ ] Every class used (e.g. `ViewModel`, `StateFlow`, `Column`, `Text`) has
      a matching import
- [ ] Every annotation used (e.g. `@Composable`, `@HiltViewModel`, `@Inject`)
      has a matching import
- [ ] Every extension function or top-level function used
      (e.g. `collectAsStateWithLifecycle()`, `viewModel()`,
      `rememberCoroutineScope()`) has a matching import
- [ ] No wildcard imports — each import is explicit
- [ ] Import grouping: Android/Jetpack/Compose → Kotlin stdlib → third-party

---

### ✅ Syntax & Compilation Checklist

- [ ] Every `{` has a matching `}`, every `(` has a matching `)`,
      every `<` (generic) has a matching `>`
- [ ] Every `when` covering a sealed class or enum is exhaustive, or has an
      `else` branch
- [ ] Every `val` and `var` is initialized before use
- [ ] No unresolved references — every name is either defined in the snippet
      or imported
- [ ] `suspend` functions are only called from a coroutine or another `suspend`
      function — call-site is shown clearly
- [ ] `@Composable` functions are only invoked from other `@Composable`
      functions or from `setContent { }`
- [ ] Lambda syntax is correct: trailing lambdas outside parentheses, explicit
      parameter names where needed for clarity
- [ ] No implicit `it` chains longer than one level — name the parameter
      explicitly beyond that

---

### ✅ Tier 1 Self-Verification

For each Tier 1 (Playground) snippet, confirm this mental test passes:

> *"If a learner opens an empty Android Studio project with a blank Compose
> Activity, and pastes this entire snippet (imports + code) into
> `setContent { }`, does it compile and run with zero errors?"*

If the answer is not a confident **yes**, revise the snippet before proceeding.

---

### ✅ Android & Kotlin Convention Checklist

- [ ] No `!!` without an inline comment explaining why it is provably safe here
- [ ] No `GlobalScope` — only `viewModelScope`, `lifecycleScope`, or a
      scoped `CoroutineScope` with defined lifecycle
- [ ] No Java-style `getX()` / `setX()` methods — Kotlin properties only
- [ ] `val` everywhere mutation is not required; `var` carries an inline
      comment explaining the need for mutation
- [ ] No deprecated APIs used without a note:
      `// Deprecated in API XX — use Y instead`
- [ ] APIs requiring a minimum SDK version are annotated: `// Requires API XX+`
- [ ] Composables: no side effects directly in the composition block —
      `LaunchedEffect`, `SideEffect`, or `DisposableEffect` used instead

---

### ✅ Labels & Comments Checklist

- [ ] Every Tier 1 snippet starts with
      `// ✏️ Tier 1 — Paste inside setContent { }`
- [ ] Every Tier 2 snippet starts with
      `// ✏️ Tier 2 — Single file, paste into your project and run`
- [ ] Every Tier 3 snippet starts with
      `// 📖 Tier 3 — Production layout (reference only)`
      followed by the file path on the next line
- [ ] Inline comments explain **why** — not what the code visibly does