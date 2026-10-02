# swarm-brainstorm

A Claude Code skill that turns one short prompt into a multi-agent brainstorm, and a worked example: **finding Jev use cases that have not been widely discussed.**

The repository holds the skill, the prompt it uses, and a complete record of one real use. The record shows what the skill does and what the model decides on its own. It lists every agent's prompt, every candidate idea, and the two ideas that were chosen.

## Live web pages

| Page | What it shows |
|---|---|
| [Brainstorm results](https://az9713.github.io/swarm-brainstorm/brainstorm-results.html) | All 35 candidate ideas from two runs, the two chosen ideas with their tests and costs, and the ranked rejected lists. |
| [Development journey](https://az9713.github.io/swarm-brainstorm/DEVELOPMENT-JOURNEY.html) | How the 14 subagents worked: stages, roles, exact prompts, who talked to whom (nobody did), and how the agent count was chosen. |

GitHub strips embedded frames from a README, so the pages open as links. Both are single HTML files in dark mode. The source files are `brainstorm-results.html` and `DEVELOPMENT-JOURNEY.html` in this repository.

## Inspiration

The skill comes from Ethan Mollick's essay [The Dot and the Swarm](https://www.oneusefulthing.org/p/the-dot-and-the-swarm) (One Useful Thing, 1 Oct 2026). In the essay, a short prompt with no team description made a model start 3 agents and organize the work itself. The skill reproduces that idea in Claude Code, where the model decides how many subagents to start and what each one does.

This project is not affiliated with Ethan Mollick or with TypeSafe AI.

## The prompt

The skill sends the prompt below. It is adapted from the prompt quoted in Mollick's essay, which named a single topic ("the next OneUsefulThing post"). It has two changes from that original:

```
Goal:
Generate candidate ideas for {TOPIC} and choose one.

Search strategy:
Explore the idea space from as many different angles as possible.

Evaluation:
Assess candidates from:
1. factual / evidentiary perspective
2. reader perspective
3. what other publications are covering

You may delegate. Organize the work as you think is best.
```

| Change | Reason |
|---|---|
| `the next OneUsefulThing post` became `{TOPIC}` | The skill takes any topic from the user. |
| The last line was added: `You may delegate. Organize the work as you think is best.` | The essay's run used a mode that delegates by itself. Claude Code has no such mode, so the permission is stated in words. |

The skill file also adds one rule: **do not define teams, roles, or agent counts.** The prompt gives the goal, the search strategy, and the evaluation views. The model decides everything else.

## Use the skill

1. Copy `SKILL.md` to `~/.claude/skills/swarm-brainstorm/SKILL.md`.
2. In Claude Code, type `/swarm-brainstorm <your topic>`.
3. The skill reports the chosen idea, the reason, the ranked rejected candidates, and the number of subagents started.

## The Jev experiment

**Jev** is the first "System One" model from [TypeSafe AI](https://typesafe.ai/blog/introducing-system-one-models-and-jev). It takes unstructured state and a typed question, and returns a typed answer (a choice among up to 255 options, a score, or a boolean) with probabilities. It never returns free text. The question for the brainstorm was: *which Jev use cases are not widely discussed?*

The same prompt ran twice with `{TOPIC}` set to that question.

| Run | Evidence base | Subagents | Candidates | Chosen idea |
|---|---|---|---|---|
| 1 | Open web research | 7 (3 + 4) | 20 | All-pairs contradiction scan of a policy corpus |
| 2 | Only the TypeSafe launch blog and the [awesome-jev](https://github.com/yibie/awesome-jev) list | 7 (4 + 3) | 15 | Dead-letter-queue triage with a replay contract |

**Run 1 winner.** Split a policy corpus into clauses. Ask Jev about each candidate pair in both orders. Flag a pair only if both orders say "contradict" with probability 0.7 or higher. Code checks numbers and dates.

**Run 2 winner.** A small consumer reads each message from a dead-letter queue and asks Jev to choose: replay, replay after a fix, quarantine, or page a human. Code lets only idempotent messages replay automatically. Jev can never discard a message. The cost estimate is $44.10 for one million messages.

Nothing was built or tested. The thresholds in the results are proposals from the agents.

## How the agents worked

- **Stages.** Each run had a gather or map stage, then an ideate stage, then a judging step done by the parent session with no agent.
- **Roles.** The model assigned each agent one angle, for example "domains nobody has covered" or "uses that depend on the probability output". The skill assigned none.
- **Communication.** No agent messaged another agent. All information passed through the parent session. In run 1, the last stage-1 agent finished after the ideators had started, so its report reached none of them.
- **The count.** The model chose 3, 4, 4, and 3 by judgment, with no formula. The journey page discloses what may have influenced the numbers.
- **Gaps.** No agent covered the "reader perspective" view. The parent judged it. Eleven of the 14 agent transcripts are empty on disk, so independence is verified for only 3 agents.

## Repository contents

| File | Purpose |
|---|---|
| `SKILL.md` | The skill, ready to install. |
| `brainstorm-results.html` | Results of the Jev brainstorm. |
| `DEVELOPMENT-JOURNEY.html` | The journey document with all 14 prompts. |
| `HANDOFF.md` | Resume notes for the next working session. |

## Caveats

- One task, two runs, one model (Claude Sonnet 5.5). The runs give no measure of variance.
- The novelty ratings are only as good as the searches behind them.
- Some entries from the awesome-jev list cited in the results are personal repositories that were not run.
