---
title: Sprin & Fall 2025 Contests
layout: page
nav_order: 2
---

{: .new-title }
> Annoucement
> - [2025-03-15] **Solution**: We have uploaded the [solution](/npc/25sp/first/solution/Editorial.pdf) for the contest.
> - [2025-03-12] **Types of verdict**: We have added the [Types of verdict](/npc/env#types-of-verdict) in the [Environment](/npc/env) page.
> - [2025-03-11] **Do's and Don't**: We have listed the [Do's and Don't](#dos-and-dont) for the contest.
> - [2025-03-10] **Detailed Environment**: We have changed the [environment](/npc/env) page to include more details. Please check it out.
> - [2025-03-08] **Penalty Calculation Updated**: Previously, the penalty was the sum of the first successful submission times for each solved problem. Now, it is the **latest** submission time among all problems where the contestant scored above $$0$$, based on the **earliest occurrence** of their highest-scoring submission.
> - [2025-03-08] We have listed the [Environment](/npc/env) for the contest. Please check it out.

# Spring & Fall 2025 Contests' Rules

The following contest rules applies for both The 1st contest, The 2nd contest and Welcome AY25/26 contest.

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

## Do's and Don't
- Contestants are only allowed to perform actions permitted within the local environemnt, such as navigating the references (details in [here](/npc/env)).
- During the contest, you are not allowed to write anything on the comments section in the contest page. If you need clarifications, you may raise your hand and ask the judge in the contest room.
- During the contest, contestants are prohibited from communicating with anyone except the judges and invigilators.
- Contestants are prohibited from submitting malicious codes, including but not limited to attacks on the judging server.
- If a contestant violates any of the aforementioned rules, the participant may be given a warning or even disqualify from the contest.

## Frequently-Asked-Question (FAQ)

1. **Are we allowed to refer any online resources or AI resources?**  
No, you are not allowed to refer to any online resources or use any AI tools such as ChatGPT, Copilot, etc.
2. **Are non-CCDS students allow to participate in the contest?**  
Yes, we allow non-CCDS students to join the contest. However, since it is our first contest, we shall focus and give more priority to CCDS students.
3. **Are we allowed to bring in any references materials (lecture slides, notes, templates, codebook, etc.)**
No, you are not allowed to bring in any materials to the contest. However, we allow certain specific materials (details in [here](/npc/env)). We will place the allowed materials in the lab computer.
4. **Why I got CE on this code?**
    ```cpp
    #include <string>

    int solve(const std::string &s) {
      return (s[0] - '0') * (s[2] - '0');
    }
    ```
    The template code is just given for your reference. You need to submit the full code. And actually, if you want, you can change the template code to your own style.
