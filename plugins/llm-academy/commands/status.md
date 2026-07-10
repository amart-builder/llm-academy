---
description: "Show your LLM Academy dashboard: rank, level, XP, streak, mastered units, what's due for review, and what's next. Read-only."
allowed-tools: Read, Glob, Grep, Bash
---

# LLM Academy: Dashboard

Show the learner a clean, motivating snapshot of where they are. Read-only: do not change any state.

## Do this

1. Resolve `STATE_DIR` by running `echo "${LLM_ACADEMY_STATE:-$HOME/.llm-academy}"`. This is mechanical: if `LLM_ACADEMY_STATE` is set, use its value verbatim. Never judge, question, or override it, and never fall back to `~/.llm-academy` while it is set; an unusual-looking path is not your concern, the user set it on purpose. Read from `STATE_DIR`:
   - `progress.json` (xp, level, rank, streak, badges, graduated, history)
   - `curriculum.json` (units and statuses, spaced-rep `nextReview` dates, track)
   - `learner-profile.json` (goal, track, preferences)
   If `learner-profile.json` is missing, tell them they have not started yet and to run `/llm-academy:start` to begin. Stop.

2. Compute from the data:
   - Path progress: count only the units required for graduation, i.e. units in `curriculum.json` with `elective` false. Show `masteredRequired / totalRequired`, so the bar can genuinely reach 100%. Electives (the `elective: true` units, like the T6 lectures) get their own separate line, e.g. `Electives: 1/3`, and never dilute the main bar.
   - Due reviews: units where `spacedRep.nextReview <= today`.
   - Next available unit: the first unit with status `available`.
   - Rank and next rank from these XP thresholds: Curious Human 0, Vector Apprentice 250, Gradient Descender 600, Backprop Ninja 1200, Attention Head 2200, Latent Space Navigator 3600, Zero-to-Hero 5500. Level = `floor(xp / 200) + 1`. (These thresholds and the level formula mirror learning-engine SKILL.md; if they ever differ, that file is authoritative.) Zero-to-Hero also requires graduation, not XP alone: if `xp >= 5500` but `graduated` is false, show the rank as Latent Space Navigator with a note that Zero-to-Hero unlocks at graduation.
   - For the XP bar, show progress within the current rank: fill = `(xp - currentRankFloor) / (nextRankFloor - currentRankFloor)`, and label it `<xp> XP (<nextRankFloor - xp> to <nextRank>)`. If already at the top rank, show the bar full.

3. Render a dashboard like this (fill with real values, keep the ASCII bars honest):

```
╭─ LLM ACADEMY ────────────────────────────────────────────╮
   Alex · explainer track · Rank: Vector Apprentice · Level 2 · 🔥 3-day streak

   XP    ██████████░░░░░░░░░░  320 XP (280 to Gradient Descender)
   Path  ████░░░░░░░░░░░░░░░░  3 / 14 required units mastered (21%)
   Electives: 0/3

   Goal: Understand how ChatGPT works, end to end.
╰───────────────────────────────────────────────────────────╯

📅 Due for review today (2)
   • u1: everything becomes numbers
   • u2: the dot product

🎯 Up next
   • u3: what is a neural network

🏅 Badges: first-vector

▸ Type /llm-academy:start to start today's session.
```

4. Adjust to their vibe preference: if `minimal`, drop the XP/streak/badge lines and show just path progress, due reviews, and next up. If `full-game`, lean into the celebration.

5. One honest, specific line of encouragement based on their recent history (from `progress.json` -> `history`). No filler.

Keep it to one screen. This is a glance, not a report.
