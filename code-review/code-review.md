---
description: Review code changes from a specific commit and report issues grouped by severity, with consequences and solutions. Use this whenever the user wants a code review, quality check, or wants to find potential bugs in recent commits.
argument-hint: commit title or keyword (e.g. `Update card UI`, `INF-345`)
allowed-tools: Bash, Read, Grep, Glob
---

# Code Review by Commit

Review the code changes introduced by a specific commit and produce a structured report of issues, grouped by severity. Focus on problems that are likely **unintentional** — oversights, mistakes, or gaps in understanding — rather than deliberate design choices. In addition to catching bugs, evaluate whether the chosen implementation approach is sound.

## Arguments

- `$ARGUMENTS` — a commit title, keyword, or ticket ID used to locate the target commit.

## Workflow

### 1. Find the commit

Search for the commit matching `$ARGUMENTS`:

```
git log --all --oneline --grep="$ARGUMENTS" -n 5
```

If multiple matches are found, pick the most recent one. If no matches, try a case-insensitive search or partial match. Show the user which commit you selected.

### 2. Get the diff

```
git show <hash> -p --stat
```

Read the full diff carefully. Understand what each change does in context — don't just look at the changed lines in isolation.

### 3. Read surrounding context

For each modified file, read enough of the surrounding code to understand:
- What the function/class is responsible for
- How the changed code interacts with its callers and dependencies
- Whether the change is consistent with the patterns used elsewhere in the file

This step is essential. Reviewing a diff without context leads to false positives and missed real issues.

### 4. Identify issues (correctness)

Look for problems across these categories. The goal is to catch things the author likely didn't intend — not to nitpick style or second-guess deliberate architecture choices.

**What to look for:**
- **Bugs**: null safety gaps, off-by-one errors, race conditions, missing error handling, incorrect logic, wrong operator, swapped arguments
- **Resource issues**: leaks (unclosed streams, unremoved listeners, uncancelled coroutines), missing cleanup in lifecycle methods
- **API misuse**: calling APIs incorrectly, ignoring return values that matter, passing wrong types that compile but behave unexpectedly
- **State problems**: stale state, missing state updates, inconsistent state across related variables
- **Edge cases**: empty collections, null inputs, boundary values, concurrent access that the code doesn't guard against
- **Performance**: unnecessary allocations in hot paths, O(n²) where O(n) is straightforward, redundant network/database calls
- **Security**: hardcoded secrets, SQL injection, unvalidated input at system boundaries
- **Syntax & idioms**: non-idiomatic Kotlin that introduces real risk (e.g., `!!` where `?.` is safe, platform types unhandled) — pure style preferences belong in the approach review, not here

**What to skip:**
- Style preferences (naming, formatting, comment density) — these aren't bugs
- Deliberate architectural patterns already used consistently in the codebase
- Changes that are clearly intentional and well-understood (e.g., adding a new field to a data class)
- Theoretical concerns that require implausible conditions to trigger
- unit test

### 5. Evaluate the approach

Beyond looking for bugs, evaluate whether the implementation approach is sound. The question here is: "Is there a meaningfully better way to do this?" — not "Would I have done it differently?"

**What to look for:**
- **Non-idiomatic Kotlin**: Java-style patterns where a Kotlin idiom is clearer (e.g., manual null checks instead of `?.let`/`?:`, verbose loops where `map`/`filter`/`fold` fit, `if-else` chains where `when` is cleaner, mutable state where immutable would work, missing use of scope functions like `apply`/`also`)
- **Coroutine misuse**: wrong scope (`GlobalScope` instead of `viewModelScope`/`lifecycleScope`), missing `Dispatchers.IO` for blocking work, callbacks where `suspend` + coroutines would be cleaner, `LiveData` where `StateFlow` fits the use case better, missing structured concurrency
- **Android architecture violations**: business logic in Activity/Fragment instead of ViewModel, direct repository/network calls from UI layer, ignoring lifecycle (e.g., observing without `viewLifecycleOwner` in Fragments), holding Context references that leak
- **Compose-specific issues** (if applicable): unstable lambdas causing recomposition, state read in wrong scope, missing `remember`, mutable state not hoisted properly
- **Reinventing existing solutions**: writing manual logic for something Jetpack/Kotlin stdlib already provides (e.g., `SavedStateHandle`, `DataStore`, `kotlinx.collections`)
- **Poor separation of concerns**: a single function doing too much, classes with mixed responsibilities, tight coupling that should be an interface
- **Testability problems**: hard dependencies on `Context`, `System.currentTimeMillis()`, static singletons that make the code untestable without instrumentation

**The bar for reporting an approach issue:**
- The alternative is **meaningfully better** — clearer, safer, more testable, more performant, or more aligned with Android/Kotlin guidelines — not just different.
- You can articulate **why** the alternative is better in one sentence. If you can't, it's a preference, not an issue.
- The improvement is **proportionate to the change**. Don't suggest rewriting the architecture because of a 10-line commit.

**What to skip:**
- Personal style preferences ("I'd use `also` here instead of `apply`")
- Refactors that go beyond the scope of the commit
- Patterns the codebase already uses consistently — even if there's a "better" way, consistency has value
- Theoretical improvements with no concrete benefit

### 6. Classify severity

Group each issue into one of these severity levels:

- **Critical** — Will cause crashes, data loss, security vulnerabilities, or corruption in production. These need to be fixed before merging.
- **Major** — Likely to cause bugs, incorrect behavior, or significant performance problems under normal usage. Should be fixed.
- **Minor** — Edge case issues, small inefficiencies, or things that could become problems as the code evolves. Worth fixing but not blocking.
- **Suggestion** — Improvements that would make the code more robust or readable but aren't bugs. Included only when the improvement is clearly worthwhile.

**For approach issues specifically:**
- **Major** — The approach causes concrete problems: untestable code, lifecycle leaks, wrong coroutine scope that will leak or crash, business logic in the wrong layer that will block future changes.
- **Suggestion** — The current approach works fine, but a cleaner/more idiomatic alternative exists. Use this for most Kotlin idiom and minor architecture observations.
- Avoid putting approach issues under **Critical** unless the approach itself will cause crashes or data loss (rare).

### 7. Produce the report

Use the exact structure below. If a severity level has no issues, omit that section entirely — don't include empty sections.

```
## Code Review: `<commit message>`

**Commit**: `<short hash>` by `<author>`
**Files changed**: <count>

---

### Critical

#### 1. <Short issue title>

**File**: `<file path>:<line number>`

**Issue**: <What's wrong — describe the specific problem in the code. Reference the actual code.>

**Consequence**: <What happens if this isn't fixed. Be concrete — "the app crashes when X" is better than "this could cause issues.">

**Solution**: <How to fix it. Be specific enough that the author can act on it without guessing, but brief — this isn't a PR, just a pointer in the right direction.>

---

### Major

#### 1. <Short issue title>
...

### Minor

#### 1. <Short issue title>
...

### Suggestion

#### 1. <Short issue title>
...

---

**Summary**: <N> issues found — <X> critical, <Y> major, <Z> minor, <W> suggestions.
```

## Important guidelines

- **Verify before reporting.** If you suspect an issue, read the surrounding code to confirm it's real. A "missing null check" that's actually guarded by the caller is noise, not a finding.
- **Be concrete.** Every issue must reference a specific file and line. Vague warnings like "consider adding error handling" without pointing to where and why are not useful.
- **Explain consequences in real terms.** "This will throw a NullPointerException when the user has no vehicles" is actionable. "This might cause issues" is not.
- **Keep solutions brief.** One to three sentences. The author knows the codebase — they need direction, not a full implementation.
- **Focus on unintentional problems.** The point of this review is to catch things the author missed, not to critique decisions they made deliberately. If something looks intentional and you're unsure, give the author the benefit of the doubt.
- **Distinguish bugs from approach.** Bugs are things that are wrong; approach issues are things that could be better. Don't conflate them — a `viewModelScope.launch` that works but uses `Dispatchers.Main` for a database query is an approach issue (Major), not a bug. Both belong in the review, but framed correctly so the author understands what they're being asked to change.
- **No false positives.** It's better to report 3 real issues than 3 real issues buried under 7 questionable ones. If you're not confident something is a problem, leave it out.
