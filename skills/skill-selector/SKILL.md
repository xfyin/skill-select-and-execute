---
name: skill-selector
description: >-
  Selects the most suitable skill from all installed skills based on user input.
  Invoke via "/skill-selector" or when the user asks which skill to use,
  "recommend a skill for this task," "what skill fits my request," or describes
  a task and wants the agent to auto-pick and run the best matching skill.
  Always use this skill when skill selection or recommendation is requested.
---

# Skill Selector

Selects the most suitable skill from all installed skills based on user input. If multiple skills match, lists the top 5 with score and a brief conclusion for the user to choose from; then runs the selected skill.

## Invocation

- **Slash command**: User invokes this skill by typing **`/skill-selector`** (or the platform equivalent for running a skill by name).
- This skill must be installed by the user; at run time the user enters the skill name (e.g. `/skill-selector`) to execute it.

## When to Use

- User explicitly asks which skill to use, or to recommend a skill.
- User describes a task and wants the agent to auto-pick and run the best matching skill.
- User invokes `/skill-selector` (or equivalent).

## Data Source

Read **available_skills** (or the equivalent list) from the current context. Each entry has `name`, `description`, and `fullPath`. Use only this information for matching and scoring; do not assume skills that are not listed.

## Execution Flow

### 1. Understand User Intent

- **Explicit selection**: User asks to pick or recommend a skill → use the task description they give (or ask for it) as the match target.
- **Implicit selection**: User gives a task and wants "the best skill" to run it → use that task description as the match target.

If the user only says "pick a skill" with no task, briefly ask: "What task or scenario do you want to accomplish?" then match against their reply.

### 2. Score Each Skill (0–100)

For each entry in **available_skills**:

- Score relevance of its **description** (and name if needed) to the match target.
- Consider: keyword/domain match, task type match, how well "when to use" in the description aligns with user intent; higher score for better fit.
- Score low (e.g. 0–30) for skills that are clearly irrelevant.

You do not need to output every score; use them internally to rank.

### 3. Decide and Output

**Case A: Single clear winner**

- Condition: Top score ≥ 80 and gap to second place ≥ 20.
- Action: State the recommendation concisely, then **list the selected skill by name** (see below), then proceed to "Run the selected skill."

**Case B: Multiple close candidates**

- Condition: Case A does not apply (e.g. top two scores are close, or top score < 80).
- Action:
  - List the **top 5** (by score, highest first). Each line: **Rank | skill name | score | one-line conclusion** (why it fits or does not).
  - Say: "Please choose which skill to use (e.g. reply with the number or skill name)."
  - After the user chooses, **list the selected skill by name** (see below), then proceed to "Run the selected skill."

**Listing the selected skill**

- As soon as a skill is chosen (either automatically or by user), output a clear line:
  - **Selected skill: \<skill name\>**
  where you substitute the exact skill name from the list (e.g. **Selected skill: pdf**). Do this before reading or executing the skill.

### 4. Run the Selected Skill

- After the user has chosen (or the single winner is chosen):
  - Use the **Read** tool to load that skill's SKILL file (its `fullPath`, usually `SKILL.md`).
  - Follow that skill's instructions to perform the user's task (or the task they give after choosing).
- If the user only wanted a recommendation and not execution, stop after the recommendation or list; do not read or run another skill.

## Output Format

- **Single recommendation**: Short recommendation + reason (1–2 sentences), then **Selected skill: \<name\>** (the actual skill name), then run it.
- **Multiple candidates**: Table or numbered list, each line: **Rank | name | score | one-line conclusion**; then prompt for choice. After choice: **Selected skill: \<name\>** (the actual skill name), then run it.

Keep analysis brief; avoid long paragraphs.

## Top-5 List Example

```
Top 5 matching skills — please choose one (reply with number or skill name):

1 | pdf          | 92 | Strong match for "extract tables from PDF"; description covers PDF handling
2 | docx         | 65 | Handles documents but focused on Word; weaker for PDF
3 | xlsx         | 45 | Tables can be exported to Excel; not the primary use case
4 | copywriting  | 20 | Focused on copy; not relevant to extraction
5 | form-cro     | 15 | Form conversion; not relevant to this task
```

## Notes

- Only choose from the installed/available skill list in the current context (e.g. agent_skills); do not invent skills.
- If no skill fits the user's intent, say so clearly ("No skill in the current list is a strong match"), briefly explain why, and suggest rephrasing or installing a more relevant skill.
