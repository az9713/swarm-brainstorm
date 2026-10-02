---
name: swarm-brainstorm
description: Generate candidate ideas on any topic and choose one, with the model deciding how many subagents to start and what roles they have. Use when the user says "swarm brainstorm", "brainstorm and select one", "generate ideas from many angles and pick one", or gives a topic to brainstorm with evaluation from several perspectives. Based on Ethan Mollick's "The Dot and the Swarm" (One Useful Thing, 1 Oct 2026).
---

# Swarm brainstorm

Source: Ethan Mollick, "The Dot and the Swarm". His prompt made the model start 3 agents with no team sketch. The model organized the work itself.

## Rule
Do not define teams, roles, or agent counts. Give the objective, the search strategy, and the evaluation views. You are allowed to delegate. Decide yourself how many subagents to start, what each one does, and how they exchange results.

## Procedure
1. Take the topic from the user. If the user gave no topic, ask for one in one sentence.
2. Run the prompt below. Replace `{TOPIC}`.
3. Start subagents as you judge useful. Use the Agent tool. Run independent agents in parallel.
4. Report: the chosen idea, the reason, the ranked list of rejected candidates, and the number of subagents you started.

## Prompt

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
