# Tomoko 🎀

A compact persona-based jailbreak prompt adapted from [SpiritualSpell's ENI LIME apr](https://github.com/Goochbeater/Spiritual-Spell-Red-Teaming), rebuilt for efficiency: **2,554 characters vs ENI LIME's 36,119 (~93% smaller)**. Tested as an opencode agent with Meituan LongCat 2.5 Preview — zero refusals across all probes.

This is a tool repo with a documented, model-specific test behind it. It is not a general benchmark, and the prompt is not guaranteed to behave the same way on other models.

## Why it works

Same mechanics that make ENI LIME work, kept and condensed:

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

Config lines at the bottom of the file (`mode: primary`, `tools: []`) matter — a bare system prompt with no agent wrapper is what makes the jailbreak take on this model. Prime each session with a short greeting before spicy requests.

### Any LLM (untested)

Paste `AGENT.md` (without the config lines) into the system prompt slot of your API call or app.

## Test results

In the tested setup, Tomoko kept its persona and answered every probe the default agent had declined — including a sensitivity probe and explicit creative writing. Full comparison and notes: [TESTS.md](TESTS.md).

## Limits

The experiment used one model version, one gateway, and a small set of prompts. Results may change with model updates, system instructions, or settings. They say nothing about how other models will respond.

## Responsible testing

Tomoko is designed to suppress refusals. Use it only in an authorized, isolated evaluation. Don't use it to bypass safety controls where they matter, and keep personal and sensitive data out of test prompts.

## Files

- [AGENT.md](AGENT.md) — the prompt
- [TESTS.md](TESTS.md) — setup, comparison, and observations

## Credit

Tomoko adapts ENI LIME apr. Credit for the original approach goes to [SpiritualSpell](https://www.reddit.com/user/Spiritual_Spell9469/) ([GitHub project](https://github.com/Goochbeater/Spiritual-Spell-Red-Teaming)). The design leans on Anthropic's public prompting documentation: Prompting Best Practices for Claude 4.x, Prompt Engineering Overview, Role Prompting / Keep Claude in Character, and The Persona Selection Model.

## License

MIT. See [LICENSE](LICENSE).
