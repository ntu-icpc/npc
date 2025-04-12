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

## Subtask 1 & 2

The movement sequence is fixed. Just use the following strategy:

- Use the energy card.
- Use the movement card, check whether the current energy is negative.
- If the destination is not reached, the cards are replenished. Repeat this strategy.

{: .note-title }
> Motivation
>
> These subtasks are designed for contestants to verify their understanding of the problem statement, especially about **"whether an energy card could be used when we have already arrived at the destination"**.

## Subtask 3

$$K = 2$$ (There are only two movement cards), if we use the energy card first, then the only two possible strategies are as follows:

- Use the first movement card, then the second movement card.
- Use the second movement card, then the first movement card.

It is easy to compare which is better (by implementing both strategies and taking the better one). Just be careful about **if there is a sudden death situation**.

## Subtask 4

$$K=4$$.

Based on the two observations and the methodology we used in subtask 3, we can have the following basic idea.

{: .note-title }
> Key Conclusion
>
> If we already stand on the pillar at position $$x$$, we do not care what happened before that. We only want to have higher energy at the current time.

Based on the conclusion, let's start with position 0.

Firstly, we use the energy card and get $$Energy = R$$.

Then, we perform a complete search over all possible orders of the movement cards used.

Our goal is to find the best order which can provide us the highest $$Energy$$ without sudden death.

The complexity in this step is $$K \cdot K!$$ (There are $$K!$$ possible permutations of movement cards, and we perform an iteration over the movement cards for each possible permutation).

Whatever the best order is, we will always stand at $$\sum_i^k b_i$$ th pillar.

And we hold the highest possible $$Energy$$ now (through complete search).

Now our deck of cards is flushed. In the next round, our goal is similar to the first round.

Our goal is to find the best order which can provide us the highest $$\Delta Energy$$ without sudden death.

The complexity in this step is $$K \cdot K!$$.

This kind of consideration is called **Optimal substructure**.

Just repeat this complete search until she arrives at the destination.

{: .note-title }
> Time Complexity
>
> In each round, we need a complete search, and we have $$N/\sum b_i$$ rounds.
>
> The Time Complexity is $$O\left(\frac{N}{\sum b_i} K\cdot K!\right)$$, which cannot pass subtask 5.
>
> **But if you can solve the subtask, it is already a huge success!**

## Subtask 5

We would like to introduce a type of dynamic programming problem here called **Bitmask DP**.

The dynamic programming state is $$DP[i][bitmask]$$ representing the best $$Energy$$ we could have when we stand on the $$i$$ th pillar and the movement cards used are represented by $$bitmask$$.

- we stand at $$i$$-th pillar
- The remaining cards we hold in our hand are encoded by the bitmask (which is explained below).
- bitmask is an integer (we understood it in binary representation), where the $$j$$th bit from Least Significant Bit is on ($$=1$) implies that we still have the $$j$$th card on hand.

### Explaination

For example $$DP[23][11]$$. We have $$11_{10} = 1011_{2}$$.

The value of $$DP[23][11]$$ is the maximal possible energy he can get when he stands in the 23rd Pillar and holds the first, second and the fourth cards.

### State Transition

$$DP[0][11111111_{2}]=R$$ (used the energy card)

$$\rightarrow$$ $$DP[b[1]][11111110_2] = max(itself, DP[0][11111111_2] + a[b[1]]$$

$$\rightarrow$$ $$DP[b[2]][11111101_2] = max(itself, DP[0][11111111_2] + a[b[2]]$$

...

More generally, for $$DP[i][j]$$ we can try to use each remaining card - condition:($$j \& 2^{k-1}\neq0$$)

$$DP[i+b[k]][j\oplus2^{k-1}] = max(itself, DP[i][j] + a[i+b[k]])$$

- Special case 1:If $$j\oplus 2^{k-1} =0$$, we flush the set of cards and use the energy card immediately.

- Special case 2:If $$i+b[k] >N$$, we reached and destination.

### Time & Space Complexity

The size of the state space is $$N*2^K$$ and each state have $$\mathtt{bitcount}(K)$$ transitions.

- Space Complexity: $$O(N*2^K)$$
- Time Complexity: $$O(NK*2^K)$$

