---
name: compact-prep
description: "Use when someone asks to prepare prompt for compact, prep for compact, compact prep, save context before compact, or wants to summarize the conversation before compacting."
---

## What This Skill Does

Generates a conversation summary and a ready-to-paste re-entry prompt before the user compacts the chat. Supports three levels of automation based on the user's message.

## Level Detection

Analyze the user's message to determine which level they want:

- **Level 1 (prep only)**: User says things like "prep for compact", "compact prep", "save context before compact", "prepare prompt for compact". No mention of actually running compact or pasting.
- **Level 2 (prep + compact)**: User says things like "prep for compact then compact", "compact prep and compact", "prep and run compact". They mention both prepping AND running compact, but do NOT mention pasting/running the prompt after.
- **Level 3 (prep + compact + paste)**: User says things like "prep for compact, compact, and paste", "prep then compact then paste and run", "do the full compact cycle". They explicitly mention pasting or running the prompt after compacting.

**Tie-breaking rules:**
- If unsure whether Level 1 or 2 → go with Level 1.
- If unsure whether Level 2 or 3 → go with Level 2.
- When in doubt at any level → always pick the lower level.

## Steps

### All Levels — Generate the prep (Level 1)

1. **Review the full conversation** — scan everything discussed so far:
   - What task(s) brought the user here
   - Key decisions made and why
   - Constraints, requirements, or preferences stated
   - Files read, created, or modified
   - Current progress (what's done, what's in-flight, what's not started)
   - Blockers, open questions, or unresolved issues
   - Any plans or task lists that are active

2. **Write a brief conversation summary** — display it under a `## Conversation Summary` heading. Keep it factual and concise (bullet points). This is for the user to review, not to paste anywhere.

3. **Generate the re-entry prompt** — write a self-contained prompt under a `## Re-entry Prompt` heading, wrapped in a single fenced code block so the user can copy-paste it directly after compacting. The prompt must:
   - State what we were working on (the goal)
   - List key decisions and constraints already established
   - List files that were touched or are relevant
   - Describe current progress — what's done and what remains
   - State the immediate next step to resume work
   - Be written as an instruction to Claude (second person: "We were working on...", "Continue by...")
   - Be compact — aim for 10-25 lines, never exceed 40 lines

4. **If Level 1**: Tell the user: "Review the re-entry prompt above. Copy it, then compact. Paste it as your first message to pick up where we left off." — then STOP.

### Level 2 — Run compact

5. After displaying the summary and re-entry prompt, tell the user: "Running compact now..." Then execute the `/compact` command to compact the conversation.

   **STOP here for Level 2.** After compact finishes, tell the user: "Compacted. Paste the re-entry prompt above as your next message to resume."

### Level 3 — Paste and run the re-entry prompt

6. After compact completes, immediately paste the re-entry prompt that was generated in step 3 and execute it as if the user had typed it. This resumes the conversation with full context.

## Output Format (Level 1)

```
## Conversation Summary

- [bullet point summary of what happened]
- ...

## Re-entry Prompt

\```
[copy-pasteable prompt here]
\```

Review the re-entry prompt above. Copy it, then compact. Paste it as your first message to pick up where we left off.
```

## Notes

- Do NOT ask the user any questions before generating — just analyze the conversation and produce the output immediately.
- If there's an active task list / plan, include the task statuses in the re-entry prompt.
- If specific files are central to the work, list their full paths in the re-entry prompt.
- The re-entry prompt should be dense with context but readable — no fluff.
- Do NOT include conversation pleasantries or meta-commentary in the re-entry prompt.
- If the conversation was trivial (e.g., a single quick question already answered), say so and note that compacting probably won't lose anything important.
- At the start of execution, briefly state which level was detected (e.g., "Detected: Level 2 — prep + compact") so the user can course-correct if needed.
