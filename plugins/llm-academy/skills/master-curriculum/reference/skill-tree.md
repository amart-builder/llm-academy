# The LLM Fundamentals Skill Tree

This is the full map of what it takes to really understand how a large language model (an LLM, the kind of AI behind ChatGPT and Claude) works, taught for a smart person who does not write code. You do not need math or programming to master the core path. You need to be willing to think, explain things back, and stay honest about what you can and cannot yet put into your own words.

The `curriculum-architect` agent reads this tree plus the learner's placement, then produces a personalized, compressed path. It skips tiers the learner already owns and goes deep on the gaps. It never teaches a unit before its prerequisites are mastered.

There are 22 units across 6 tiers. Each unit has a stable `id` (u1 through u22). Mastery is defined by an observable "can explain or do X" check, never by "understands X." The recurring bar: can you explain this idea to a smart friend, in your own words, without the jargon.

---

## Two tracks

Every learner picks a track at onboarding, switchable anytime from the coach menu:

- **explainer**: understand how it all works, no coding. Builder units are left out of your path. You still reach graduation and can explain ChatGPT end to end.
- **builder**: everything, including the hands-on coding units where you build the pieces yourself (Karpathy's "Zero to Hero" code-alongs).

Each unit is tagged `track: core` or `track: builder`. Core units are taught to both tracks. Builder units are only compiled into a builder path; on the explainer track they are left out, and if the learner switches to builder later the path recompiles to include them without losing any mastery already earned. Some units are also tagged `elective: true`: electives deepen or branch off but never block anything and never gate graduation.

---

## The North Star: what "gets it" actually means here

Not "can recite the transformer paper." It means:

1. **The through-line.** Everything an LLM does starts by turning things into numbers, then does math on those numbers. You can trace that line from raw text to a next-word guess.
2. **Plain-language fluency.** You can explain embeddings, attention, training, and why models hallucinate to someone with no technical background, and they get it.
3. **Honest mental models.** You know where an LLM's "knowledge" physically lives, why it is confident when wrong, and what its limits are.
4. **The practical payoff.** You understand why confidence is not truth, how semantic search and RAG work (embeddings plus dot products), and that embeddings are cheap building blocks you can use.
5. **(builder track) You built the pieces.** You wrote a tiny autograd engine, a small language model, and a mini-GPT, so none of it is magic anymore.

The capstone (u19) is where the whole picture comes together: you teach ChatGPT back, end to end.

---

## Licensing rule (applies to every lesson built from this tree)

Teach the concepts in the academy's own words. Link learners to the original sources. Never reproduce a video transcript or an article's prose. A short attributed quote (a line or two, credited) is fine. The sources are the property of their authors; we point people to them and add our own explanation, we do not copy them.

---

## Sources

Every unit names its source material. The full registry, with links and verification status, is in `reference/sources.md`. Read it alongside this tree. When a learner or Alex supplies a new article or video, it gets appended to `sources.md` and the `curriculum-architect` slots a new unit into the right tier (see "Adding future material" at the bottom).

---

# TIER T1: Everything becomes numbers

The one idea the whole field is built on. Get this and the rest has a foundation.

### u1: Everything becomes numbers `track: core`
**The idea:** A computer cannot work with the word "cat." So we turn every word (and image, and sound) into a vector, which is just a list of numbers, like coordinates on a map. Words with similar meaning get placed near each other. This is what "embeddings" are. The famous trick "king minus man plus woman lands near queen" works because meaning became geometry. Every kind of input (text, image, audio) enters the model through this same door.
**Sources:** Raghav Dixit Part A, section "Everything becomes numbers." 3Blue1Brown Chapter 5 (first third).
**Prereqs:** none.
**Mastery check:** Explain to a smart friend why "words become arrows" and what it means for two words to be "close." No jargon they would not already know.

### u2: The dot product `track: core`
**The idea:** How do you measure whether two of those arrows point the same way? The dot product. You multiply matching numbers and add them up (a times b = sum of a_i times b_i, which also equals length times length times the cosine of the angle between them). A big positive result means "these point the same way, they are similar." This multiply-then-add is the single most common operation an LLM does, billions of times, which is why we need GPUs (chips built to do exactly this in parallel). Teaser: attention, the heart of a transformer, is this same operation at scale.
**Sources:** Raghav Dixit Part A, section on the dot product.
**Prereqs:** u1.
**Mastery check:** Compute a tiny dot product by hand (two short lists of numbers) and say what the sign and the size of the answer tell you about the two vectors.

---

# TIER T2: The machine

What a neural network actually is, stripped of mystique.

### u3: What is a neural network `track: core`
**The idea:** A neuron is just a little function: it takes some numbers in, weights them, adds them up, and passes the result on. Stack many neurons into layers, and layers on top of layers, and you get a network that can learn complicated patterns. "Weights" and "biases" are the knobs it tunes. Depth (many layers) is what lets it build simple ideas into complex ones.
**Sources:** 3Blue1Brown Chapter 1 (about 27 minutes).
**Prereqs:** u1, u2.
**Mastery check:** Walk through what one neuron computes, step by step, and say why stacking layers buys you more than one big layer would.

### u4: MLPs: reshaping space `track: core`
**The idea:** An MLP (multi-layer perceptron, the plainest kind of neural network) does this: h = f(Wx + b). In English, it takes your input, stretches and folds the space it lives in, over and over, until data that was tangled together gets pulled apart into neat groups. All of the network's "knowledge" physically lives in those weight numbers W and the biases b. When people say GPT is made of "MLP blocks," this is what they mean.
**Sources:** Raghav Dixit Part A, section on the machine that reshapes space.
**Prereqs:** u3.
**Mastery check:** Explain, in plain words, where a trained network's "knowledge" actually sits, and what "reshaping space" is doing for us.

### u5: Activation functions `track: core`
**The idea:** The f in that formula is the activation function, and it is what makes the whole thing work. Without it, stacking layers is pointless: a stack of straight-line steps is still just one straight line. The activation adds a bend, so the network can learn curves. History in one breath: the old "sigmoid" bend caused "vanishing gradients" (the learning signal faded to nothing in deep networks and froze the field for years); "ReLU" fixed it with a dead-simple bend; today's models use smoother versions (GELU, SwiGLU).
**Sources:** Raghav Dixit Part A, section on activation functions.
**Prereqs:** u4.
**Mastery check:** Explain what breaks if you remove the activation function, and what sigmoid's failure (vanishing gradients) actually was.

---

# TIER T3: Learning

How a network goes from random to smart. This is training.

### u6: What "wrong" means: loss `track: core`
**The idea:** Before a model can improve, it needs a number that says how wrong it is right now. That number is the "loss." For language models it is cross-entropy: L = minus log of the probability the model gave to the correct next word. The log matters: being confidently wrong is punished far harder than being unsurely wrong. Training is just: make this number small. This is also the seed of why models hallucinate: a fluent wrong answer and a fluent right answer can look equally confident.
**Sources:** Raghav Dixit Part A, section on loss.
**Prereqs:** u3, u4, u5.
**Mastery check:** Given two model predictions for the same word, say which one has the bigger loss and why, and connect that to why confidence is not the same as being right.

### u7: Gradient descent `track: core`
**The idea:** Picture the loss as a landscape of hills and valleys, where low ground means "less wrong." The "gradient" points in the direction of steepest uphill: the fastest way to get MORE wrong. To improve, we step the opposite way, downhill, and that is exactly why the update subtracts it: theta becomes theta minus eta times the gradient (eta is the "learning rate," the step size). Too big a step and you overshoot; too small and it takes forever. Drop the minus sign and you climb toward more of something instead of less, which is exactly how models are later trained to chase rewards.
**Sources:** 3Blue1Brown Chapter 2 (about 20 minutes). Raghav Dixit Part A, section on rolling downhill.
**Prereqs:** u6.
**Mastery check:** Narrate one full training step, start to finish, in plain words: measure wrongness, find the downhill direction, take a step, repeat.

### u8: Backprop, intuitively `track: core`
**The idea:** Backpropagation is how the network figures out which knob to turn and by how much. Think of it as blame flowing backward: the final error gets traced back through the layers, and each weight learns how much it contributed to the mistake, so it knows which way to nudge. No calculus needed to get the picture: it is a chain of influence running in reverse.
**Sources:** 3Blue1Brown Chapter 3 (about 15 minutes).
**Prereqs:** u7.
**Mastery check:** Explain how "blame" flows backward through the layers so every weight learns which way to move.

### u9: Backprop calculus `track: builder`
**The idea:** The formal version of u8. The chain rule (from calculus) is the machinery that makes backprop exact: partial derivatives tell you precisely how a small change in one weight changes the final loss. This unit is for learners who want the real math under the intuition.
**Sources:** 3Blue1Brown Chapter 4 (about 10 minutes).
**Prereqs:** u8.
**Mastery check:** Derive the gradient for a simple 2-node chain by hand, showing the chain rule at work.

### u10: Code: build micrograd `track: builder`
**The idea:** You build a tiny "autograd" engine from scratch: a small piece of software that does backprop automatically on single numbers. Once you have written it yourself, backprop stops being magic. This is Karpathy's famous first lecture and the doorway to the whole builder path.
**Sources:** Karpathy "Zero to Hero" lecture 1 (2h25m).
**Prereqs:** u8 (u9 recommended, not required).
**Mastery check:** A working micrograd-style `Value` class with a `backward()` method that computes gradients correctly. The evaluator may run your code to confirm.

---

# TIER T4: Language models

Where the numbers-and-training machine becomes something that produces language.

### u11: Next-word prediction `track: core`
**The idea:** A language model is, at heart, one thing: a machine that outputs a probability for every possible next word. Give it "The cat sat on the," and it produces "mat: 40%, floor: 12%, roof: 3%..." Then it picks one (the "temperature" setting controls how boldly or safely it picks), adds it to the sentence, and repeats. That loop is how ChatGPT writes. "It is just autocomplete at scale" is both fair and unfair, and you should be able to say why.
**Sources:** 3Blue1Brown Chapter 5 (middle section). Concepts from Karpathy lecture 2.
**Builder mode:** the bigram makemore code-along, where you build a tiny name-generating model (Karpathy lecture 2, 1h57m). Completing the code-along is what "u11-code" refers to in the builder full-clear requirement.
**Prereqs:** u6, u7, u8.
**Mastery check:** Explain where the next word ChatGPT writes actually comes from, and give one honest sentence on why "just autocomplete" both fits and misses.

### u12: Code: makemore MLP and training diagnostics `track: builder`
**The idea:** You build a small neural language model (an MLP over characters) and, just as important, learn to read its health: are the activations and gradients flowing well, is the loss curve behaving, what does BatchNorm do. This is the craft of actually training a model instead of just describing one.
**Sources:** Karpathy lectures 3 (1h15m) and 4 (1h55m).
**Prereqs:** u11.
**Mastery check:** A trained character-level MLP language model, plus you can read its loss curve aloud and say what it means. Evaluator may run it.

### u13: Code: backprop ninja and WaveNet `track: builder, elective: true`
**The idea:** For the learner who wants full rigor: do backprop by hand through the whole MLP (no autograd safety net), then build a deeper, hierarchical model (WaveNet-style convolutions). Marked elective: it is extra depth, not on the critical path.
**Sources:** Karpathy lectures 5 (56m) and 6.
**Prereqs:** u12.
**Mastery check:** Manual backprop through the MLP that matches the autograd result, and a working hierarchical (WaveNet-style) model.

### u14: Tokenization `track: core`
**The idea:** Models do not see words, they see "tokens," which are word-chunks (sometimes a whole word, often a piece of one). The rule that decides the chunks is called BPE (byte-pair encoding). This sounds like a footnote but it explains a shocking amount of weird LLM behavior: why models miscount letters, fumble arithmetic, and struggle with some non-English text. The quirks of the tokenizer leak into the model's mistakes.
**Sources:** Karpathy lecture 8 (2h13m). Explainer track: watch as a concept, no coding. Builder track: full code-along (this is what "u14-code" refers to).
**Prereqs:** u11.
**Mastery check:** Explain two real, observable LLM failures (for example miscounting letters in a word, or bad arithmetic) and trace each back to how tokenization works.

---

# TIER T5: Transformers and the payoff

The architecture behind modern LLMs, and the moment it all clicks.

### u15: What is a GPT `track: core`
**The idea:** The full guided tour of a GPT, in order: text becomes tokens, tokens become embeddings (those number-vectors from u1), the embeddings flow through a stack of "blocks," a final step ("unembedding") turns them back into scores for every possible next word, and "softmax" turns those scores into clean probabilities. This is the map; the next units zoom into the interesting parts.
**Sources:** 3Blue1Brown Chapter 5 (full, about 27 minutes).
**Prereqs:** u11, u14.
**Mastery check:** Sketch in words the whole path of data through a GPT, naming each stage and what it does.

### u16: Attention `track: core`
**The idea:** The breakthrough. A fixed embedding for "bank" cannot tell a riverbank from a money bank; the word needs to look at its neighbors to settle its meaning. Attention is the mechanism that lets words pass meaning to each other. Each word sends out a Query ("what am I looking for?"), and every word offers a Key ("here is what I am") and a Value ("here is what I will give you if you pick me"); the dot product from u2 scores the matches, softmax turns them into a focus pattern, and meaning flows accordingly. "Masking" keeps a word from peeking at the future; "multi-head" runs several of these in parallel.
**Sources:** 3Blue1Brown Chapter 6 (about 26 minutes). Pending slot: Raghav Dixit Part B when published (see sources.md).
**Prereqs:** u15.
**Mastery check:** Using a concrete sentence, explain how attention moves meaning between words. Show where the dot product from u2 shows up.

### u17: Code: build GPT from scratch `track: builder`
**The idea:** You build the whole thing: a working decoder transformer, in code, that generates text. Everything from the earlier builder units comes together here. This is Karpathy's "Let's build GPT."
**Sources:** Karpathy lecture 7 (1h56m).
**Prereqs:** u12, u16.
**Mastery check:** A working mini-GPT that generates text. The evaluator may run it.

### u18: How LLMs store facts `track: core`
**The idea:** Where does "Michael Jordan plays basketball" actually live inside the model? Not in the attention, but in the MLP blocks, which act like a giant key-value memory: a pattern comes in, a stored fact comes out. This unit also covers rough parameter counting (how the billions of numbers add up) and "superposition" (how a model crams more concepts than it has neurons by overlapping them).
**Sources:** 3Blue1Brown Chapter 7 (about 23 minutes).
**Prereqs:** u16.
**Mastery check:** Answer, in plain words, "where does an LLM keep the fact that Michael Jordan plays basketball?" and explain why it is the MLP blocks, not attention.

### u19: Capstone: explain ChatGPT end to end `track: core`
**The idea:** No new material. You teach the whole pipeline back, in your own words: text becomes tokens, tokens become embeddings, attention moves meaning around, MLP blocks add stored knowledge, a final layer produces scores (logits), sampling picks the next word, and the whole thing was trained by the loss-and-gradient loop from Tier T3. You also cover why it hallucinates, plus the three practical takeaways from Raghav Dixit Part A: (1) confidence is not truth, (2) semantic search and RAG are just embeddings plus dot products, (3) embeddings are cheap building blocks you can call as an API. Passing this is graduation.
**Sources:** none new. Draws on all prior core units and Raghav Dixit Part A.
**Prereqs:** all other core units (u1, u2, u3, u4, u5, u6, u7, u8, u11, u14, u15, u16, u18).
**Mastery check:** Deliver a clear, correct, jargon-free walkthrough of the entire pipeline plus the three practical takeaways. Graded by the evaluator against a rubric: coverage (did you hit every stage), correctness (no wrong claims), and clarity (a smart non-technical person would follow it).

---

# TIER T6: Frontier

Expert chalkboard lectures from the Dwarkesh Podcast (launched April 2026), the "whiteboard explainers." Every unit here is elective, available on both tracks, and unlocked once u15 and u16 are mastered. They never block graduation. These are long and meaty, so the coach frames each one the same way: watch the lecture, then come back and we debrief and pressure-test what you took away. There is no "teach it in chat" mode for these, because the lecture itself is the teaching; mastery is a discussion where you defend what you learned.

### u20: How LLMs are actually trained and served `track: core, elective: true`
**The idea:** The economics and engineering of real frontier models: batch sizes, how a mixture-of-experts model is laid out across many chips, and why API prices are what they are. Connects back to u2's "why GPUs."
**Sources:** Reiner Pope chalkboard lecture (about 2h14m). Video and essay links in sources.md.
**Prereqs:** u15, u16.
**Mastery check:** Explain, in plain terms, why serving a frontier model costs what it does.

### u21: RL from AlphaGo to LLMs `track: core, elective: true`
**The idea:** Reinforcement learning: AlphaGo, tree search (MCTS), self-play, and how RL for language models both borrows from and differs from RL for board games. Ties directly to u7's "drop the minus sign and climb toward reward" and to how a next-word predictor becomes a helpful assistant.
**Sources:** Eric Jang chalkboard lecture (about 2.5h). Links in sources.md.
**Prereqs:** u15, u16.
**Mastery check:** Explain self-play and why RL for LLMs is harder than RL for Go.

### u22: Chips from the bottom up `track: core, elective: true`
**The idea:** Hardware from the ground up: logic gates building up to GPUs and TPUs. Pairs with u2's "why GPUs" and closes the loop from math down to silicon.
**Sources:** Reiner Pope chalkboard lecture 2 (about 1h20m). Links in sources.md.
**Prereqs:** u15, u16.
**Mastery check:** Trace a multiply-add operation from a single logic gate all the way up to an attention head.

---

## The prerequisite graph

```
T1:  u1 ─> u2
T2:  (u1,u2) ─> u3 ─> u4 ─> u5
T3:  (u3,u4,u5) ─> u6 ─> u7 ─> u8 ─> [u9] ─> [u10]
T4:  (u6,u7,u8) ─> u11 ─> [u12] ─> [u13(elective)]
                    u11 ─> u14
T5:  (u11,u14) ─> u15 ─> u16 ─> u18 ─> u19(capstone: needs all core)
                          u16 ─> [u17]
T6:  (u15,u16) ─> u20(elective), u21(elective), u22(elective)
```

Units in [brackets] are `track: builder`. Electives are marked. A unit is `available` when its listed prereqs are all mastered. Default prereq is the previous core unit in the same tier; the first unit of a tier requires all core units of the tier before it. Electives and builder units never appear as prereqs of a core unit, so leaving them out (explainer track) never blocks the core path.

---

## Graduation

- **Standard graduation (either track):** every core unit mastered (u1, u2, u3, u4, u5, u6, u7, u8, u11, u14, u15, u16, u18) plus the capstone u19 passed. The T6 electives are not required.
- **Builder-track "full clear":** standard graduation plus u10, u11 in code mode (u11-code), u12, u14 in code mode (u14-code), and u17.

Graduation is what unlocks the top rank, Zero-to-Hero. It is a real bar (you can explain the whole thing), never an XP number alone.

---

## Compression rules for the architect

Placement scores the learner 0 to 5 per tier (T1 through T5). Apply per tier:

- Tier at level 4 or 5: skip the teaching, replace each of its core units with one quick confirmation check. If passed, mark mastered.
- Tier at level 3: teach only the gaps, not every unit in full.
- Tier at level 0 to 2: full teaching, every core unit.
- T6 is never placement-scored. Its units are always optional electives, offered once u15 and u16 are mastered.
- Explainer track: omit every `track: builder` unit at compile time. Builder track: include everything.
- Never place a unit before its prereqs. Electives never block. Compression means cutting what the learner already owns, never lowering the mastery bar.

---

## Adding future material

This tree is meant to grow. When Alex or a learner supplies a new article or video (for example Raghav Dixit Part B on attention, or another Dwarkesh lecture), the flow is:

1. Append the source to `reference/sources.md` with its link and status.
2. Ask the `curriculum-architect` to slot a new unit (or extend an existing one) into the right tier, with prereqs, a track tag, an elective flag, and an observable mastery check in the same style as the units above.
3. Recompile the affected learner path without disturbing any already-mastered units.

Two slots are already anticipated: Raghav Dixit Part B feeds into u16 (attention) when published, and any further Dwarkesh lectures become new T6 electives.
