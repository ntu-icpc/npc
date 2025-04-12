---
title: D - Game
layout: page
parent: Solutions
---

# D - Game

## Abridged Problem Statement

Given an array that consists of integers. Use the set of cards to move to get the maximal sum.

Subtasks:

- For all task, $$1 \leq N \leq 10^5, 1\leq R \leq 10^9, 0 \leq |a| \leq 10^9$$
- Subtask 1 & 2: $$K = 1$$
- Subtask 3: $$K = 2$$
- Subtask 4: $$K = 4$$
- Subtask 5: $$K = 8$$

## Observations

{: .note-title }
> Observation 1
> 
> The total distance moved is fixed until the set of movement cards is flushed.

{: .note-title }
> Observation 2
> 
> Always use the energy card as soon as possible.
