# LLM Academy

A Claude Code plugin that teaches you how LLMs actually work. It places you with a short assessment, builds a path around what you already know, and gates every concept behind "can you explain this in your own words." Made for smart people who have never written a line of code, with an optional track for people who want to build.

It teaches the starter list people keep recommending for climbing the AI learning curve:

- [3Blue1Brown's Neural Networks series](https://www.3blue1brown.com/topics/neural-networks) (intuition, beautifully visual)
- [Andrej Karpathy's Neural Networks: Zero to Hero](https://github.com/karpathy/nn-zero-to-hero) (build everything from scratch in code)
- [Dwarkesh Patel's chalkboard lectures](https://www.dwarkesh.com) (how frontier LLMs are trained, served, and run on real hardware)
- [Raghav Dixit's "Vectors are all you need"](https://x.com/_raghavdixit_/status/2074930760155312172) (the fundamentals as a crisp read)

The academy teaches every concept in its own words and links you to the originals. It never reproduces them. All credit for the source material goes to the authors above.

## Install

```bash
claude plugin marketplace add amart-builder/llm-academy
claude plugin install llm-academy@llm-academy
```

Restart Claude Code, then run `/llm-academy:start`.

## What you get

- A placement conversation instead of a fixed course: it finds what you already know and skips it.
- Two tracks: the explainer track (understand it all, no code) and the builder track (adds all 8 Karpathy code-along lectures, where your code actually gets run and graded).
- 22 units across 6 tiers: from "everything becomes numbers" to attention, transformers, and how GPT stores facts, capped by one final exam: explain ChatGPT end to end, in your own words.
- Mastery checks (85% to pass), spaced reviews that come back the day you'd start forgetting, XP, ranks, and streaks.
- It grows: when new material ships, `/llm-academy:start add-source` slots it into your path.

See [plugins/llm-academy/README.md](plugins/llm-academy/README.md) for full docs.

## License

MIT. The plugin's own text is original; the videos and articles it links to belong to their authors.
