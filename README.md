# Guided Design Coach

A Claude skill that coaches you through design problems with questions, instead of handing you the answer.

## Why

I asked Claude a design question once. Its answer was great, and much better than mine. That felt bad for a minute, and then it raised a better question: if I always get the answer ready-made, when do I actually think?

Two problems:

1. I wasn't engaging with the problem anymore. I read the answer and moved on.
2. The answers looked good, but I didn't know the reasoning behind them. A solution you don't understand is a risk: on the day it stops working, you won't know why.

So I built this skill with Claude. Now, when I bring a design problem, it asks me questions step by step, like a coach, and I reach the answer myself. When I get there, I also know why.

## What it does

- Asks one focused question at a time and builds on your answers.
- Walks through a clear process: **Problem → Goals & constraints → Unknowns → Options → Trade-offs → Design → Stress test → Summary**, and tells you which phase you're in so you learn the structure.
- When you're stuck, gives the smallest useful hint first, and escalates only if needed.
- Names trade-offs instead of declaring right answers, and points out flaws as scenarios ("What happens when two users do X at the same time?").
- Gives you the full answer whenever you ask for it, and shows which of your own ideas it builds on.

## Example

> **You:** I need to design a notification system for our app.
>
> **Coach:** *(Problem)* Before anything else: in one or two sentences, what problem do notifications solve for your users, and why now?
>
> **You:** Users miss important updates, like when someone replies to them.
>
> **Coach:** So it's about timely awareness of replies. *(Goals & constraints)* What must the system do, and what must it never do? For example, is a 5-minute delay acceptable?

## Install

**Claude Code:** copy `guided-design-coach/SKILL.md` to `~/.claude/skills/guided-design-coach/SKILL.md` (or `.claude/skills/guided-design-coach/SKILL.md` inside a project).

**Claude.ai / Claude app:** zip the `guided-design-coach` folder and upload it as a skill in Settings → Capabilities → Skills.

Then just ask something like *"Coach me through designing X"* or *"Don't give me the answer, guide me."*

## License

MIT
