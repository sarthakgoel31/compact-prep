# Compact Prep

**Save your conversation context before Claude Code resets it.**

## What It Does

Compact Prep generates a comprehensive conversation summary and a copy-pasteable re-entry prompt before you compact your Claude Code context window. When a long conversation hits the context limit, compacting clears the history -- and without prep, you lose all the decisions, file paths, progress tracking, and nuance built up over the session. This skill captures everything into a dense re-entry prompt so you can resume exactly where you left off.

## How It Works

1. You say "/compact-prep" or "prep for compact"
2. The skill detects which automation level you want:
   - **Level 1 (prep only):** Generates the summary and re-entry prompt. You copy it, compact manually, and paste it back.
   - **Level 2 (prep + compact):** Generates the prep, then runs `/compact` automatically. You paste the re-entry prompt after.
   - **Level 3 (prep + compact + paste):** Full cycle -- generates prep, compacts, and immediately pastes the re-entry prompt to resume work.
3. The skill scans the entire conversation for:
   - What task(s) brought you here
   - Key decisions made and why
   - Constraints, requirements, and preferences stated
   - Files read, created, or modified (with full paths)
   - Current progress (done, in-flight, not started)
   - Blockers, open questions, and unresolved issues
   - Active task lists or plans
4. Two outputs are produced:
   - A **Conversation Summary** (bullet points for your review)
   - A **Re-entry Prompt** (a self-contained instruction block, 10-25 lines, wrapped in a code fence for easy copy-paste)

## Key Features

- **Three automation levels:** From manual copy-paste to fully automatic prep-compact-resume
- **Conservative level detection:** When ambiguous, the skill picks the lower level to avoid unintended compaction
- **Dense re-entry prompts:** 10-25 lines covering goal, decisions, constraints, file paths, progress, and next step -- no fluff
- **Task list preservation:** If you had an active task list or plan, statuses are included in the re-entry prompt
- **Trivial conversation detection:** If the conversation was a single quick question, the skill says so instead of generating unnecessary prep
- **Immediate execution:** No questions asked before generating -- the skill analyzes the conversation and produces output right away

## Usage

```
/compact-prep                          # Level 1: generate prep, you handle the rest
prep for compact                       # Same as above
prep for compact then compact          # Level 2: generate prep + run compact
prep for compact, compact, and paste   # Level 3: full automatic cycle
save context before compact            # Level 1
```

**Trigger phrases:** "compact prep", "prep for compact", "save context before compact", "prepare prompt for compact"

## Output Format

```
## Conversation Summary

- We were building feature X for project Y
- Decided to use approach A because of constraint B
- Modified files: /path/to/file1.ts, /path/to/file2.py
- Completed: auth flow, database schema
- Remaining: API integration, testing

## Re-entry Prompt

    We were building [feature] for [project]. The goal is [outcome].

    Key decisions:
    - [decision 1 and why]
    - [decision 2 and why]

    Files touched: [paths]
    Current state: [what is done, what remains]
    Next step: [exactly what to do next]

Review the re-entry prompt above. Copy it, then compact.
Paste it as your first message to pick up where you left off.
```

---

Built with [Claude Code](https://claude.ai/code) as a slash command skill.
