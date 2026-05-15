# LLM Reasoning Atlas

> An interactive decision system for AI-augmented product management.

**Live → https://denis-hamon.github.io/llm-reasoning-atlas/**

Pick the right LLM reasoning pattern before you prompt. The atlas recommends a reasoning chain, checks context quality, and gives you a copy-ready prompt with the right prompt-engineering pattern.

## What's inside

- **12 technique families** — Ideation, Discovery, Strategy, Prioritization, Experimentation, Metrics, UX, Competition, Delivery, Monetization, GTM, Risk
- **148 techniques** — each with principle, use case, prompt kernel, and engineering boost
- **High-yield chains** — 8 multi-step reasoning recipes for complex decisions
- **17 LLM-native moves** — Tree of Thoughts, ReAct, Reflexion, RAG, Multi-agent debate, and more
- **Copy-ready prompts** — includes context-quality-gate and output requirements

## Claude Code Skill — `/atlas`

The `/atlas` skill routes any PM situation to the right reasoning technique and delivers a copy-ready structured prompt — directly in your terminal or IDE.

### Install

```bash
mkdir -p ~/.claude/skills/atlas
curl -o ~/.claude/skills/atlas/SKILL.md \
  https://raw.githubusercontent.com/Denis-hamon/llm-reasoning-atlas/main/skills/atlas/SKILL.md
```

### Usage

```
/atlas Our activation rate dropped 60% over the last 3 weeks.
       Users sign up but leave before reaching the first key action.

→ routes to: Funnel diagnosis + Segmented diagnosis chain
→ pairs with: Reflexion loop (LLM tool)
→ delivers: copy-ready causal prompt with context-quality-gate
```

The skill covers 10 PM situation types: `discover`, `prioritize`, `validate`, `differentiate`, `improve`, `launch`, `strategy`, `decision`, `measure`, `llm-tool`.

## Contributing

Contributions from product managers are very welcome.

### How to contribute a technique

1. Fork the repo
2. Edit `src/pages/ProductReasoningAtlas.jsx`
3. Add your technique to the right category in the `categories` array:
   ```js
   ['Technique name', 'One-line principle.', 'Best used for.', 'Prompt kernel.']
   ```
   Or add to `categoryExpansions` (bonus techniques per family).
4. Open a pull request with a short description of when and why to use the technique.

### Guidelines

- The prompt kernel must be actionable (a real instruction, not a description)
- Prefer techniques grounded in existing PM/strategy/research practice
- Each technique should be genuinely different from existing ones
- Keep names short (2–4 words)
- For new LLM tools: cite the research paper or origin

### What we're looking for

- Underrepresented domains (AI product, platform, enterprise, hardware)
- Better prompts for existing techniques
- New high-yield chains
- Translations (FR, DE, ES…)
- New routing rules for the `/atlas` skill

## Tech stack

React + Vite, deployed via GitHub Pages. No backend, no dependencies beyond React.

## License

MIT — free to use, adapt, and share.
