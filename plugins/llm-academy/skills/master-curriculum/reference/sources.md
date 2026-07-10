# LLM Academy: Sources Registry

Every unit in the skill tree is anchored to public source material. This is the master list. Links were checked on 2026-07-10. When a link is marked VERIFIED, the coach can hand it to a learner with confidence. When it is PENDING, the material is announced but not yet public, so the coach teaches the concept and notes the source is coming.

**Licensing rule (hard):** these sources belong to their authors. We teach the concepts in our own words and point learners to the originals. We never reproduce transcripts or article prose. Short attributed quotes (a line or two) are fine.

---

## Primary sources (the Lieberman starter list)

| Source | What it is | Link | Status |
|---|---|---|---|
| 3Blue1Brown, Neural Networks series | 7 videos, Chapters 1 to 7, animated and plain-language | https://www.3blue1brown.com/topics/neural-networks | VERIFIED 2026-07-10 |
| Karpathy, Neural Networks: Zero to Hero | 8 code-along lectures plus the code repo | https://github.com/karpathy/nn-zero-to-hero | VERIFIED 2026-07-10 |
| Raghav Dixit, "Vectors are all you need" (Part A) | Long-form article, the plain-English backbone of Tiers T1 to T3 | https://x.com/_raghavdixit_/status/2074930760155312172 | VERIFIED 2026-07-10 (via mirror) |
| Raghav Dixit, Part A (mirror) | Same article, LinkedIn mirror | https://www.linkedin.com/pulse/vectors-all-you-need-raghav-dixit-fop8c | VERIFIED 2026-07-10 |
| Raghav Dixit, Part B (attention and transformers) | Announced follow-up, not yet published | (none yet) | PENDING |
| Dwarkesh Patel, chalkboard lectures | Expert board lectures, the T6 "whiteboard explainers" | https://www.dwarkesh.com | VERIFIED 2026-07-10 |

The Raghav Dixit Part A primary link is the X (Twitter) article. X blocks automated fetching, so it was verified through the LinkedIn mirror, which resolves and carries the same piece ("Vectors are all you need" by Raghav Dixit, published 2026-07-08). Both links are live; use whichever the learner prefers.

---

## 3Blue1Brown chapters (mapped to units)

The series page above lists all seven chapters. Per-video links are not pinned here because the series page is the stable home and the chapter order is what matters. Learners open the series page and pick the chapter named in each unit.

- Chapter 1: But what is a neural network? -> u3
- Chapter 2: Gradient descent, how networks learn -> u7
- Chapter 3: Backpropagation, intuitively -> u8
- Chapter 4: Backpropagation calculus -> u9 (builder)
- Chapter 5: how LLMs and GPTs work / transformers -> u1 (first third), u11 (middle), u15 (full)
- Chapter 6: Attention in transformers -> u16
- Chapter 7: How LLMs store facts -> u18

Series home: https://www.3blue1brown.com/topics/neural-networks

---

## Karpathy, Zero to Hero lectures (mapped to units)

The repo above is the stable home and links every lecture video plus its notebook. Lecture order (verified against the repo 2026-07-10):

1. The spelled-out intro to neural networks and backpropagation: building micrograd -> u10 (builder)
2. The spelled-out intro to language modeling: building makemore (bigram) -> u11 builder mode
3. Building makemore Part 2: MLP -> u12 (builder)
4. Building makemore Part 3: Activations and Gradients, BatchNorm -> u12 (builder)
5. Building makemore Part 4: Becoming a Backprop Ninja -> u13 (builder, elective)
6. Building makemore Part 5: Building WaveNet -> u13 (builder, elective)
7. Let's build GPT: from scratch, in code, spelled out -> u17 (builder)
8. Let's build the GPT Tokenizer -> u14

Repo home: https://github.com/karpathy/nn-zero-to-hero

---

## Dwarkesh Patel chalkboard lectures (Tier T6, all elective)

Launched April 2026. Expert-taught board lectures. Each has a YouTube video and a companion essay on dwarkesh.com. All verified 2026-07-10.

| Unit | Lecture | Video | Essay |
|---|---|---|---|
| u20 | Reiner Pope, how LLMs are trained and served (~2h14m) | https://youtu.be/xmkSf5IS-zw | https://www.dwarkesh.com/p/reiner-pope |
| u21 | Eric Jang, RL from AlphaGo to LLMs (~2.5h) | https://www.youtube.com/watch?v=X_ZVSPcZhtw | https://www.dwarkesh.com/p/eric-jang |
| u22 | Reiner Pope, chip design from the bottom up (~1h20m) | https://www.youtube.com/watch?v=oIk3R-sMX5o | https://www.dwarkesh.com/p/reiner-pope-2 |

Extra (no unit, offered as optional enrichment inside T6): Sasha Rush, a 13-minute impromptu board clip on on-policy self-distillation: https://youtu.be/wxOZWD6wYVY

---

## Adding a source

When new material arrives (a learner shares it, or Alex drops a link):

1. Add a row here with the source, what it is, the link, and a status (VERIFIED with today's date if you checked it resolves, else PENDING).
2. Tell the `curriculum-architect` to slot a unit into the right tier (see the skill tree's "Adding future material").
3. Never invent a link. If you cannot verify one, mark it PENDING and say so.
