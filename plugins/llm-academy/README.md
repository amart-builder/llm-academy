# LLM Academy

An adaptive, mastery-based academy that runs inside Claude Code and takes you from "I have no idea how ChatGPT works" to "I can explain the whole thing, end to end, in my own words." Built for smart non-coders. No math or programming background needed for the core path.

It places you, builds a personalized and compressed path from four excellent public resources, and teaches with the learning methods that actually work fast: mastery learning (the Alpha School model), active recall, spaced repetition, and learning by teaching. You never advance past something until you can explain it to a smart friend.

The four sources it teaches from: 3Blue1Brown's Neural Networks series, Andrej Karpathy's "Neural Networks: Zero to Hero," Raghav Dixit's "Vectors are all you need," and Dwarkesh Patel's chalkboard lectures. The academy teaches the concepts in its own words and links you to the originals; it never reproduces them.

## Install

LLM Academy is a Claude Code plugin. From your terminal:

```bash
claude plugin marketplace add amart-builder/llm-academy
claude plugin install llm-academy@llm-academy
```

(Working from a local clone instead? `claude plugin marketplace add /path/to/llm-academy` works the same way.)

Restart Claude Code so the plugin loads, then run `/llm-academy:start`. (Plugin commands are namespaced, so it is `/llm-academy:start`, not a bare `/llm-academy`.)

### The cache and version-bump trap (read this if you edit the plugin)

Claude Code installs a plugin into a local cache under `~/.claude/plugins/cache`, keyed by the plugin's `version` in `.claude-plugin/plugin.json`. If you change any plugin file (a lesson, the skill tree, a command) but do not bump that version number, Claude Code keeps serving the old cached copy and your change appears to do nothing. So whenever you edit the plugin, bump the `version` in `plugin.json` (for example 0.1.0 to 0.1.1) and restart Claude Code. This only matters for people developing the plugin; ordinary learners never touch it. Your progress is stored separately (see below), so bumping the version never affects it.

## How to use

- `/llm-academy:start`: the one command you need. First time, it places you, asks whether you want the explainer track or the builder track, and builds your path. After that, it runs your daily session. It also takes shortcuts: `/llm-academy:start review`, `/llm-academy:start next`, `/llm-academy:start reassess`, `/llm-academy:start settings`, `/llm-academy:start add-source`.
- `/llm-academy:status`: a quick glance at your rank, XP, streak, what is due for review, and what is next.

A learning session happens in a single Claude Code session and is fully interactive. Your progress is saved between sessions, so you pick up exactly where you left off.

Every unit gives you two doors: watch or read the original source (with the link and length up front), then come back and we dig in, or have the coach teach it right there in chat. Both land on the same mastery check. Builder-track coding units add a third door: build it live, file by file, with the coach able to run your code.

## The two tracks

At onboarding you choose one, and you can switch anytime by running `/llm-academy:start settings` or just asking the coach mid-session:

- **explainer**: understand how it all works, no coding. The builder-only units are left out. You still graduate and can explain ChatGPT end to end.
- **builder**: everything, including the hands-on Karpathy code-alongs where you build a tiny autograd engine, a small language model, and a mini-GPT yourself.

Switching tracks recompiles your path (adding or setting aside the builder units) and never erases progress you have already earned.

## What's inside

**Commands** (what you type)
- `/llm-academy:start`: the coach and router. Places you, then runs daily mastery-paced sessions.
- `/llm-academy:status`: the dashboard.

**Agents** (the non-interactive staff the coach delegates to)
- `assessor`: designs your short placement and grades it into a level per tier.
- `curriculum-architect`: compiles your personalized, compressed, dependency-ordered path, and recompiles it on a track switch.
- `lesson-builder`: authors each unit's lesson, tuned to you, when you want it taught in chat.
- `evaluator`: grades your mastery checks, runs your code on builder units, and judges the capstone, honestly.

**Skills** (the brains)
- `learning-engine`: the teaching method: the source-anchored loop, mastery gates, spaced repetition, gamification, tracks, graduation, and the voice rules. Includes the state schema.
- `master-curriculum`: the 22-unit LLM fundamentals skill tree, the source registry, and the rules for compiling the tree down to your path.

## Where your progress lives

`~/.llm-academy/`
- `learner-profile.json`: who you are, your track, your level per tier, your preferences.
- `curriculum.json` / `curriculum.md`: your path (machine and human-readable).
- `progress.json`: XP, level, rank, streak, badges, graduation status.
- `sessions/`: a short journal per learning day.

This is separate from the plugin code, so updating or reinstalling the plugin never touches your progress. Want it synced across machines or backed up? Set the `LLM_ACADEMY_STATE` environment variable to a folder inside your cloud-synced directory and the academy stores everything there instead.

## Adding new material

The academy is designed to grow. When new material comes out (Raghav Dixit's Part B on attention, another Dwarkesh lecture, anything worth teaching), run `/llm-academy:start add-source` and give the coach the link. It verifies the link, adds it to the source registry, and asks the curriculum architect to slot a new unit into the right tier with its own prereqs and mastery check, without disturbing anything you have already mastered.

## Design notes

- The main Claude Code session is your live coach. It does all the talking and adapts in real time. It spawns the agents above for the heavy lifting (placing, planning, authoring, grading) to keep the conversation sharp and the work specialized.
- Mastery is always observable: "can explain X in your own words," never "understands X." Compression comes from skipping what you already own, never from lowering the bar.
- Tuned for a smart non-coder: analogies first, equations second and always translated into English, no jargon left undefined. The recurring test is whether you can teach the idea to someone else.
