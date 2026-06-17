\---

description: Create summary for PRs

argument-hint: (optional) a branch name, ticket ID (e.g. `TOL-318`), or empty for the current branch

allowed-tools: Read, Write, Bash

\---



\# PR Summary Generator



Generate a structured pull request description for the current branch by analyzing every commit's diff against the base branch.



\## Arguments



\- `$ARGUMENTS` — (optional) a branch name, ticket ID (e.g. `TOL-318`), or empty for the current branch.



\## Workflow



\### 1. Resolve the target branch



\- If `$ARGUMENTS` is empty, use the current branch (`git branch --show-current`).

\- If `$ARGUMENTS` looks like a ticket ID (e.g. `TOL-318`), find the matching branch: `git branch -a | grep <ticket-id>`. Use the local branch if it exists, otherwise the remote tracking branch.

\- If `$ARGUMENTS` is an exact branch name, use it directly.



\### 2. Extract the target ticket ID



Extract the ticket ID \*\*from the resolved branch name\*\*, not from commit messages.

Branch names typically follow the pattern `feature/TOL-318-some-description` or `TOL-318-some-description`.

The ticket ID is the uppercase-letters-plus-digits segment (e.g. `TOL-318`, `PK-3968`).



> This is the \*\*primary ticket ID\*\* for the PR. All subsequent steps use it as a filter.



\### 3. Determine the base branch



Run `git merge-base develop <target-branch>` to find the fork point. If `develop` doesn't exist, try `main`. This is the comparison baseline.



\### 4. Collect and filter commits



```

git log <base>...<target> --oneline --reverse

```



Use `--reverse` so commits appear in chronological order (oldest first).



\*\*Filter the list immediately:\*\* keep only commits whose message starts with the primary ticket ID extracted in Step 2. Discard any commits belonging to a different ticket (e.g. parent or dependency tickets bundled in the branch). Save only the filtered commit hashes and messages.



If no commits match the primary ticket ID, fall back to all commits in the range and note the ambiguity.



\### 5. Inspect every filtered commit



For \*\*each\*\* filtered commit:



1\. `git show <hash> --stat` — to get the file-change summary.

2\. `git show <hash> -p -- ':(exclude)\*.json' ':(exclude)\*.lottie' ':(exclude)\*Test.kt' ':(exclude)\*Spec.kt' ':(exclude)\*Tests.kt'` — to get the full diff, excluding large generated files and all test files.

3\. If the stat shows large generated files that were excluded, note them briefly (e.g. "Added Lottie animation `boom\_gate.json`") but do not analyze their raw content.

4\. If the stat shows \*\*only\*\* test files changed (nothing left after exclusions), \*\*skip this commit entirely\*\* — do not include it in the output.



Read the diffs carefully. Understand what changed structurally — new classes, moved code, changed signatures, added dependencies, new patterns.



\### 6. Produce the PR description



Use the exact structure below. Do not add extra sections, footers, or boilerplate.



\### 7. Save the output



1\. Ensure the output directory exists: `mkdir -p .claude/pr\_summary`

2\. Use `cat <<'EOF' > .claude/pr\_summary/<TICKET-ID>.md` via Bash to write the final PR description. Do \*\*not\*\* use the Write tool — use Bash to avoid triggering the edit-approval prompt.

3\. Print a short confirmation: `PR summary saved to .claude/pr\_summary/<TICKET-ID>.md`



Do \*\*not\*\* print the full PR description to the terminal — the terminal's markdown renderer consumes backtick formatting. The file preserves all markdown exactly as written for pasting into GitHub.



```

\## \[TICKET-ID] — PR Description



\### Summary



\[Two to four bullet points. Describe the PROBLEM or GAP that existed before this branch — what was broken, missing, or suboptimal and why it matters.

For bug fixes, include a "Root cause:" sub-bullet that identifies exactly where the logic was wrong.

End with one bullet that states the high-level solution — what the ticket does to address the problem, in a single sentence. Keep it at the "what", not the "how".]



\### Solution



\#### Commit 1 — `<full commit message>`



\- \[Bullet: WHAT changed and WHY. Name the specific class, function, pattern, or architectural decision.]

\- \[Another bullet for a logically distinct change within the same commit.]



\#### Commit 2 — `<full commit message>`



\- ...

```



\## Writing guidelines



These guidelines keep the description accurate and easy to paste directly into a GitHub PR.



\*\*Line length — critical for GitHub paste:\*\*

\- Every bullet point must be written as a \*\*single unbroken line\*\* with no manual line breaks or indented continuations.

\- Never split a sentence across two lines with an indented continuation (the style `- Long sentence\\n  continues here` breaks GitHub rendering when copied from terminal).

\- If a bullet would be very long, tighten the wording rather than wrapping it.



\*\*Backtick formatting — mandatory:\*\*

\- Wrap every code identifier in backticks: class names, function names, variable names, constants, file names, XML attributes, enum values, annotation names, data keys.

\- This applies everywhere — Summary bullets, Solution bullets, and commit headings.

\- Example: `` `EvChargingSessionPinFragment` ``, `` `setupView()` ``, `` `screenHasFingerprintButtonInKeypad` ``, `` `newInstance(...)` ``.

\-

\*\*Summary section:\*\*

\- Use bullet points, not prose paragraphs.

\- Describe the existing incorrect/missing behaviour first, then (for bugs) add a "Root cause:" bullet.

\- Keep it to 2–4 bullets, with the final bullet always being the high-level solution.

\- The final bullet must summarise what the ticket does to fix or add — one sentence, high-level only (the "what", not the "how"). Detailed implementation belongs in the Solution section.

\- Do not reference other unrelated flows, screens, or features as context (e.g. "inconsistent with Deal Purchase, Auto Top-up, DuitNow…").

\- Do not include user-impact statements ("users were forced to…", "this confused users…") — describe the technical broken state only.



\*\*Solution section (per commit):\*\*

\- Each bullet should explain the implementation choice, not just what file changed. `` "Added `hasActiveVehicle` guard to `BeaconScanService` start" `` is better than `"Modified MainApplication.kt"`.

\- Focus on architectural decisions, new patterns, domain logic changes, and non-obvious tradeoffs.

\- Skip trivial changes (import reordering, whitespace, formatting) unless they signal something meaningful (e.g. removing an unused dependency).

\- Group related changes within a commit logically (e.g. all permission-related changes together, then all layout changes).

\- If a commit is large, prioritize the 5–8 most important non-test changes rather than listing everything.

\- Stop each bullet at the core claim. Do not append parenthetical elaborations — boolean expressions spelling out the full condition, implementation internals (e.g. `stateIn(WhileSubscribed(5000))`), or "removing the X step" cleanup notes. State the WHAT and the immediate WHY, then stop



\*\*Test code — excluded entirely:\*\*

\- Do not describe test classes, test functions, test helper utilities, or test coverage in either the Summary or Solution sections.

\- If a commit contains both production and test changes, only describe the production changes.

\- If a commit contains only test changes, omit that commit from the output completely.



\*\*Tone:\*\*

\- Concise and technical. No filler, no "this commit also…" phrasing.

\- Write for a reviewer who understands the codebase but hasn't seen these changes yet.



\## Example



Branch: `feature/PK-3968-multi-monthly-pass-hidden-greyed` (primary ticket: `PK-3968`).

The branch also contains bundled `PK-3860` commits — those are \*\*ignored\*\* because they don't match the primary ticket ID.



```

\## PK-3968 — Enhance multi monthly pass UI to support hiding and greying-out options



\### Summary



\- `ParkingMonthlyPassDetailFragment` uses only the `enabled` field to determine whether a pass option is shown or hidden, so disabled options disappear from the list instead of appearing greyed-out.

\- The backend introduced a new `isHidden` field but the client DTO and filtering logic were never updated to handle it.

\- This ticket adds `isHidden` support to the client DTO and updates the pass list filtering to distinguish between hidden and disabled options.



\### Solution



\#### Commit 1 — `PK-3968 Enhance multi monthly pass ui to support hiding and greying-out options`



\- `NextMultiMonthlyPass.isHidden: Boolean?` added to the client DTO with `@SerializedName("isHidden")`, nullable to handle older API responses that omit it.

\- `multiPassAdapterList` in `ParkingDetailViewModel` now filters `isHidden == true` items out entirely while keeping `enabled == false` items in the list so they render as greyed-out. Null or false `isHidden` is treated as visible.

```



\---



Second example — bug fix branch `fix/TOL-318-ev-charging-biometric`:



```

\## TOL-318 — Fix biometric auth not triggered on EV charging PIN screen



\### Summary



\- On `EvChargingSessionPinFragment`, the fingerprint button was not shown in the PIN keypad and the system biometric prompt was not triggered automatically on screen entry, even when the user had an encrypted PIN registered.

\- Root cause: `EvChargingSessionPinFragment` never overrode `setupView()` to call `navigationListener.showFingerprintAuth(...)`, and `newInstance(...)` never set `screenHasFingerprintButtonInKeypad = true`, so the base `EnterPinFragment` skipped both the auto-prompt and the keypad button.

\- This ticket overrides `setupView()` in `EvChargingSessionPinFragment` and sets the missing keypad flag so biometric auth behaves consistently with other PIN screens.



\### Solution



\#### Commit 1 — `TOL-318 Fix biometric auth not triggered on EV charging PIN screen`



\- `EvChargingSessionPinFragment.setupView()` override added — when `biometricHelper.hasEncryptedPin` is `true`, calls `navigationListener.showFingerprintAuth(...)` under the `TYPE\_PAYMENT` flow so the system fingerprint prompt appears immediately on screen entry.

\- `screenHasFingerprintButtonInKeypad = true` set in `newInstance(...)` to enable the fingerprint icon inside the PIN keypad.

```

