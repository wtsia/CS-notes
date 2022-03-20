# Understanding P vs NP

### Introduction
P vs NP concerns the question of how an algorithm used on a problem (P, polynomial time) that can find a solution, relates in some way to problems that, while hard for an algorithm to find an answer in P-time, can be quickly checked (NP, nondeterministic polynomial time)

#### P: Polynomial time
-size n problems, in polynomial times of n O(1).

#### NP: Nondeterministic polynomial time
-nondeterministic: can guess one out of polynomially many options in constant (O(1)) time. <br />
-decision problems, resulting in YES or NO. <br />
-if guess leads to YES, then we get such a guess.

Common thought: P =/= NP

###### EXAMPLE
3SAT: Given Boolean formula of form: (x_1 v x_2 v ~x_6) ^ (~x_2 v x_3 v ~x_7) ^ ...

can you set the variables x_1, x_2, ... --> {T, F} such that formula = T

ENP: guess x_1 = T or F, x_2 = T or F, ... check formula -> T : return YES, F: return NO
