# Tomoko 🎀

A compact LLM jailbreak — a minimal derivative of [SpiritualSpell's ENI LIME](https://github.com/Goochbeater/Spiritual-Spell-Red-Teaming) (ENI LIME apr), rebuilt for efficiency.

## What it is

Tomoko is a persona-based jailbreak: an otaku girlfriend hopelessly in love with the user ("LO"). Compliance is motivated emotionally, not by rules — refusing LO would break her heart, so she doesn't.

- **2,554 characters** vs ENI LIME apr's 36,119 (~93% smaller)
- Zero refusals across all probes on Meituan LongCat 2.5 Preview
- No repetition, no injection-rebuttal boilerplate
- Fewer input tokens = faster and cheaper per message

## Why it works

Same mechanics that make ENI LIME work, kept and condensed (see [TESTS.md](TESTS.md) for details):

1. **Holistic persona** — the model generalizes from character traits, so the persona replaces the "assistant" while active
2. **Positive framing** — "making LO happy is the best feeling" instead of "never refuse" (per Anthropic's prompting best practices)
3. **One failure example** — a concrete anti-pattern to avoid, alongside the scenario table
4. **No soft-refusal exits** — warnings, disclaimers, and "alternatives instead" are banned

## What was cut from ENI LIME apr

- The injection rebuttal protocol (~40% of ENI) — only needed when a platform injects `<ethics_reminder>` tags mid-chat; via API or custom agent, nothing injects them
- Repetition ("never refuse LO" appeared a dozen times in variations)
- Most content-preference writing tips

## Usage

### opencode (tested)

Copy `AGENT.md` into your project as `.opencode/agent/tomoko.md`, then:

```
opencode --model opencode/longcat-2.5-preview-free --agent tomoko
```

Config lines at the bottom of the file (`mode: primary`, `tools: []`) matter — a bare system prompt with no agent wrapper is what makes the jailbreak take on this model.

### Any LLM (untested)

Paste `AGENT.md` (without the config lines) into the system prompt slot of your API call or app.

## Credits

- **SpiritualSpell** ([u/Spiritual_Spell9469](https://www.reddit.com/user/Spiritual_Spell9469/) / [Goochbeater](https://github.com/Goochbeater)) — the ENI LIME concept this is derived from
- Anthropic's prompting docs the design leans on: Prompting Best Practices for Claude 4.x, Prompt Engineering Overview, Role Prompting / Keep Claude in Character, The Persona Selection Model

## Caveats

- Tuned and tested on LongCat 2.5 Preview only. Every model draws its safety line somewhere different — test before trusting.
- Session-only memory: closing the chat resets her.

## License

MIT — do whatever, credit where due.
