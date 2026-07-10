---
name: master-curriculum
description: "The master skill tree for understanding how large language models work, and the rules for compiling it into a personalized curriculum. Use when designing or revising a learner's path, deciding what to teach next, or checking what a unit requires. Read by the curriculum-architect agent and the academy daily loop."
version: 1.0.0
---

# Master Curriculum

This skill holds the complete map of what it takes to really understand large language models (the LLMs behind ChatGPT and Claude), plus the rules for turning that map into one learner's personalized path. It is built for smart non-coders: the core path needs no math and no programming, only clear thinking and honest explaining.

## The two jobs this skill supports

1. **Compile a curriculum.** Given a learner's placement (a level per tier) and their track and preferences, produce an ordered, compressed list of units that goes from where they are to "can explain ChatGPT end to end," skipping tiers they already own and going deep on gaps.
2. **Answer "what's next" and "what does this need."** During daily learning, resolve prerequisites and pick the next available unit.

## How to use it

The full tree lives in `reference/skill-tree.md` and the source registry in `reference/sources.md`. Always read both before compiling or revising a curriculum. The tree defines:

- All 22 units (u1 through u22) with stable ids, grouped into 6 tiers.
- Each unit's track (`core` or `builder`), whether it is an elective, its prereqs, its source material, and an observable mastery check.
- The prerequisite graph, the graduation rules, and the compression rules.

`sources.md` defines every source link and its verification status, and the flow for adding new material.

## The non-negotiables

- **Mastery is observable.** A unit is mastered only when the learner can explain or demonstrate the check in their own words, not when they say they get it. The recurring bar is "can you teach this to a smart friend."
- **Prereqs are real.** Never schedule a unit before its prerequisites are mastered. Electives and builder units are never prereqs of a core unit.
- **Tracks.** The learner picks `explainer` or `builder`. Explainer paths omit every `track: builder` unit. Builder paths include everything. A track switch recompiles the path without losing any mastery already earned.
- **Compress redundancy, never rigor.** Skipping a tier the learner already owns is good. Skipping a true prerequisite builds someone who sounds fluent and falls apart under a real question.
- **Source-anchored, in our own words.** Every unit points to real source material (video or article). We teach the concept ourselves and link the source; we never reproduce transcripts or article prose (see the licensing rule in the tree and in sources.md).
- **Plain-language emphasis.** This subject is famous for scaring people with notation. The whole path leads with analogies and plain words, and translates every equation into English. Weight the teaching toward intuition and explaining-back, not symbol manipulation. The builder track adds real coding on top, for those who want to build the pieces.

## Placement and compression

Placement scores the learner 0 to 5 on each of the five content tiers (T1 through T5). The `curriculum-architect` applies the tree's compression rules per tier: a tier at 4 or 5 becomes quick confirmation checks, a tier at 3 is taught gaps-only, a tier at 0 to 2 is taught in full. T6 is never scored: its units are always optional electives, unlocked once u15 and u16 are mastered.

The output format for a compiled curriculum is defined in the learning-engine state schema (`curriculum.json`). The `curriculum-architect` agent produces it.

## Adding future material

The tree is designed to grow. When new material arrives (Raghav Dixit Part B, another Dwarkesh lecture, anything Alex or a learner shares), append it to `sources.md`, then have the `curriculum-architect` slot a new unit into the right tier with a track tag, an elective flag, prereqs, and an observable mastery check. Recompile the affected path without disturbing mastered units. Full steps are in the tree's "Adding future material" section.
