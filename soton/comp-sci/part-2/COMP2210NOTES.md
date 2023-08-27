# Theory of Computing Cheat Sheet

## Regular Languages Closure
- RLs are closed under union, intersection, complement, reversal, concatenation, and Kleene star.

## Context-Free Languages Closure
- CFLs are closed under union, concatenation, and Kleene star.
- CFLs are also closed under intersection with a RL.

## Recursively Enumerable Sets Closure
- A set A is r.e. if it is accepted by a TM.
- R.E. sets are closed under union, intersection, and concatenation.

## Recursive Sets Closure
- A set A is recursive if it is accepted by a total TM.
- Recursive sets are closed under union, intersection, complement, and concatenation

## Known Recursive Sets - Proofs found in Lecture 20
DFA = `{ <B,w> | B is a DFA that accepts input string w }`

DFA = `{ <A> | A is a DFA and L(A) = Ø }`

DFA = `{ <A,B> | A, B are DFAs and L(A) = L(B) }`

CFG = `{ <G,w> | G is a CFG that generates string w }`

CFG = `{ <G> | G is a CFG and L(G) = Ø }`

## Turing Reduction Theorems
- If A ≤ B and B is r.e. then so is A. Equivalently, if A is not r.e. then B is not r.e. 
- If A ≤ B and B is recursive then so is A. Equivalently, if A isn’t recursive then neither is B.

## The Class P Closure
- The class P is the class of languages that can be decided in polynomial time.
- The class P is closed under union, intersection, concatenations, and complement.

## The Class NP Closure
- The class NP is the class of languages that can be verified in polynomial time.
- The class NP is the class of languages that can be decided by a non-deterministic Turing machine in polynomial time.
- The class NP is closed under union, intersection and concatenation.

## The Class EXPTIME
EXPTIME = $\bigcup_{k\in\mathbb{N}} TIME(2^{n^{k}})$

## Polynomial Time Reductions
- If A $\leqslant_p$ B and B ∈ P, then A ∈ P.
- If A $\leqslant_p$ B and A ∉ P, then B ∉ P.
- If every problem A in NP reduces to B in polynomial time, then B is NP-hard.
- If B ∈ NP as well, then B is NP-complete.
- Cook-Levin Theorem: Every NP problem has a polynomial time reduction to SAT.
- If every problem A in PSPACE reduces to B in polynomial time, then B is PSPACE-hard.
- If B ∈ PSPACE as well, then B is PSPACE-complete.

## Time Complexity
- If an algorithm f(n) is O(g(n)) for some equation g(n), that means that the algorithm’s complexity scales, at worst, with g(n), but it could do better in certain cases.
- We say that f(n) is O(g(n)) if there exists real c > 0, integer $n_0$ > 0 with f(n) <= c g(n) for all n >= $n_0$. This is the asymptotic upper bound for f(n).

## Space Complexity
- The space complexity of a decider M is the function f : N -> N where f(n) is the maximum number of tape cells M scans on any branch of its computation, on any input of length n.
- We say that M is a f(n) space TM.
- The complexity class PSPACE(f(n)) is the collection of all languages that are decidable by an O(f(n)) space deterministic Turing machine.
- The complexity class NSPACE(f(n)) is the collection of all languages that are decidable by an O(f(n)) space non-deterministic Turing machine.
- Savitch Theorem: For every f(n) space NTM, there exists a $f(n)^{2}$ space DTM $\Rightarrow$ PSPACE = NPSPACE.

## Relating Time and Space Complexity
- If a decider runs in f(n) space, then it runs in $2^{O(f(n))}$ time.
- A machine running in polynomial time can only use polynomial space.

## PSPACE and NPSPACE
- PSPACE: The set of all languages that can be determined by a deterministic TM in polynomial space.
- NPSPACE: The set of all languages that can be determined by a non-deterministic TM in polynomial space.

## Known Problems

P: PATH

NP-COMPLETE: SAT, HAMPATH, 3COL, TSP-D

PSPACE-HARD: GC, GGO

PSPACE-COMPLETE: TQBF, GG
