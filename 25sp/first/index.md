---
title: NPC25 The 1st Contest
layout: page
parent: Spring 2025 Contests
nav_order: 1
---

# NPC25: The 1st Contest

<script src="https://cdn.logwork.com/widget/countdown.js"></script>
<a href="https://logwork.com/countdown-timer" class="countdown-timer" data-style="flip3" data-timezone="Asia/Singapore" data-date="2025-03-15 14:00">
Nanyang Programming Contest 2025: The 1st Contest
</a>

{: .new-title }
> Annoucement
>
> - [2025-03-10] **Detailed Environment**: We have changed the [environment](/npc/env) page to include more details. Please check it out.
> - [2025-03-08] **Penalty Calculation Updated**: Previously, the penalty was the sum of the first successful submission times for each solved problem. Now, it is the **latest** submission time among all problems where the contestant scored above $$0$$, based on the **earliest occurrence** of their highest-scoring submission.
> - [2025-03-08] We have listed the [Environment](/npc/env) for the contest. Please check it out.

## Introduction

**Nanyang Programming Contest (NPC) 2025: The 1st Contest** is organized by the International Collegiate Programming Contest (ICPC) teams in the College of Computing and Data Science (CCDS), Nanyang Technological University (NTU). The contest aims to promote competitive programming in NTU's community. Moreover, the contest aims to train students to be better problem solvers in facing the Online Assessment (OA) during job seeking.

## Details of the Contest

- **Date**: 15th March 2025
- **Time**: 1:30PM - 5:00PM
- **Venue**: Software Project Lab (N4-B1B-11)

| Time            | Details                                |
| --------------- | -------------------------------------- |
| 1:15PM - 1:30PM | Registration                           |
| 1:30PM - 2:00PM | Briefing & Pre-contest preparation     |   
| 2:00PM - 4:00PM | Contest                                |
| 4:00PM - 5:00PM | Contest Discussion                     |

The contest discussion is conducted to discuss the solutions to the problems in the contest.

## Eligibility Requirements

The contest is open to all CCDS students.  However, if a contestant has represented NTU in ICPC in any season, the contestant is **not** eligible in this contest.

## Contest Format (Partial Scoring)

### Contest Tasks

The contest consists of 4 to 8 algorithmic problems which are to be solved by writing code. Each problem is given a problem score which is the maximum attainable score from that problem. Each problem has a time limit and memory limit, which are the maximum allowable runtime and maximum allowable memory size, respectively.

### Scoring of the contest

The contest adopts the rules known as "Partial Scoring". Each solution (code) submitted to each problem is known as a submission. The submission is tested against multiple test cases that are not revealed to the contestant. A submission passes a test case if the solution produces the correct output within the time limit and memory limit of the problem. The score of the problem for each submission is the multiplication of the problem score and the fraction of test cases passed by the submission over the total number of test cases for the problem. Mathematically, suppose the problem $$i$$ has a maximum score of $$s_i$$ and $$t_i$$ test cases. If a submission passes $$r_i$$ (which is less than or equal to $$t_i$$), the score for the submission is $$s_i \cdot \frac{r_i}{t_i}$$. The score for the problem is the maximum score out of all submissions. The score of the contestant is the **sum** of scores the contestant gets for each problem.

### Additional Remarks

We guarantee that all problems are solvable within the time limit and memory limit by the following programming languages: C, C++, Java, and python.

### Prizes

The ranking is sorted according to the following order.

- The ranking is first sorted in descending order of the score of the contestant.
- Among contestants with the same score, the ranking is sorted by penalty in ascending order. The penalty is the latest submission time among all problems where the contestant scored above $$0$$. For each problem, the highest-scoring submission's earliest occurrence is used. Mathematically, let $$s_i$$ be the score of problem $$i$$, $$t_i$$ be the first time of the submission of problem $$i$$ that has the highest score, and $$p_i$$ be the penalty of problem $$i$$. Then, the penalty of the contestant is $$ \max_{s_i>0} \left\{p_i\right\} $$.

The prizes are awarded as follows.

- Contestants in the the top $$10 \%$$ in the ranking will be awarded the gold/silver/bronze prizes.
- Contestants in the next $$10 \%$$ in the ranking will be given consolation prizes.

| Tiers           | Prizes      |
| --------------- | ----------- |
| Gold            | 100 SGD     |  
| Silver          | 75 SGD      |                                
| Bronze          | 50 SGD      |
| Consolation     | 20 SGD      |

* As it is our first time in organizing programming contest, the details are subject to changes.

## Frequently-Asked-Question (FAQ)

1. **Are we allowed to refer any online resources or AI resources?**  
No, you are not allowed to refer to any online resources or use any AI tools such as ChatGPT, Copilot, etc.
2. **Are non-CCDS students allow to participate in the contest?**  
Yes, we allow non-CCDS students to join the contest. However, since it is our first contest, we shall focus and give more priority to CCDS students.
