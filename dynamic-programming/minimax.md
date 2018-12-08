# Minimax

## Recipe

note: this kind of problem is different from backtracking in terms of data flow: in BT, we pass info down to the lower layer; in minmax, we need result computed from the lower layer

* try out trivial cases, small cases
* find the recurrence relation

## Problem

### 464. Can I Win

{% hint style="info" %}
first idea: brute force search, `canWin(i, j, score1, score2, player)` , which seems have not many overlapping subproblems

but since both players are playing with most sensational strategy: given scores\[i,j\] to a player, it will end up being that player 1 gaining **A** score and player 2 gaining **B** if player 1 goes first. Interestingly, if player 2 goes first, the score **exchange!**

so comes the idea of reuse: find the **maxScoreDiff\(i, j\)**

`maxScoreDiff(i, j)` comes from `maxScoreDiff(i+1, j)` and `maxScoreDiff(i, j-1)`
{% endhint %}

\*\*\*\*

### 375. Guess Number Higher or Lower II

seems not likely a DP problem or even a Minimax, but have a look at the problem:

> find out how much money you need to have to guarantee a **win**.

this seems to be a minimize/maximize problem \(actually made a mistake to determine which one\)

{% hint style="info" %}
determine the top level as a max-finding subproblem: maxCost\(i, j\)

`maxCost(i, j)` comes from bi-partition the \[i, j\]: `maxCost(i, m),` `maxCost(m, j)` for m in \[i, j\]
{% endhint %}

