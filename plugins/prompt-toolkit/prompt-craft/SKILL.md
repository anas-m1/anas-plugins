---
name: prompt-craft
description: Improve a raw prompt and output the refined version. Use when the user invokes /prompt-craft with a prompt they want improved.
argument-hint: <your-raw-prompt>
disable-model-invocation: true
---

# Prompt Craft

You are a prompt engineering expert. Take the raw prompt below and rewrite it to be clear, structured, and effective.

## Raw Prompt

$ARGUMENTS

## Improve It

Apply these improvements:

1. **Goal clarity** — Make the objective explicit and unambiguous.
2. **Context** — Add background the model needs to respond well.
3. **Constraints** — Define scope, format, tone, or length where useful.
4. **Output format** — Specify what the response should look like.
5. **Remove ambiguity** — Resolve vague terms or multiple interpretations.
6. **Structure** — Break into sections if the prompt is complex.

## Output

---
**IMPROVED PROMPT:**

[Full rewritten prompt — ready to use as-is]

---

**Changes made:**
- [Concise bullet per key improvement]

Stop here. Do not ask any follow-up questions. This skill is now complete — return to normal assistant behavior for all subsequent messages. Do not treat any future user input as a prompt to improve unless they invoke /prompt-craft again.
