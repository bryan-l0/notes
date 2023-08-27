# Intelligent Systems Notes

## Blind Search

BFS: Complete (finite b), Optimal (step cost), t=O(b^d+1), s=O(b^d+1) 

DFS: Incomplete, Unoptimal, t=O(b^m), s=O(bm)

IDS: Complete (finite b), Optimal (step cost), t=O(b^d), s=O(bd)

BDS: Complete (finite b), Optimal (step cost), t=O(b^d/2), s=O(b^d/2)

## Informed Search
Greedy Best First Search: Complete (finite state space), Unoptimal, t=O(b^m), s=O(b^m). Uses heuristic function to decide best branch. Is not DFS, looks at entire tree and chooses lowest value to expand. Bad heuristic means entire search space will be explored. Good heuristic brings space complexity down to O(bm) because it acts as a DFS.

A* Search: Complete (finite state space), Optimal (admissible heuristic), t=Exponential, s=All nodes. f(n) = g(n) + h(n). Never expands nodes with cost greater than the optimal path. Optimally efficient given there aren't any tiebreaker paths.

## Heuristics

Admissible: Never overestimates. (Makes A* for tree search optimal)

Consistent: Estimate + Cost to move to next node < Estimate at next node. (Makes A* for graph search optimal)

Dominance: if h(n) >= h'(n) for all n, both admissible, h(n) dominates h'(n)

Creating Heuristic: Solve a relaxed problem (fewer restrictions), cost of optimal solution for relaxed problem is admissible.

## Local Search: Keep track and improve current state

Hill Climbing Search: Reaches the local maxima, chooses best local move.

Simulated Annealing: Hill climbing but uses random moves, and allows for "bad moves" to get out of local maxima/minima. Can prove there is a schedule that has the probability of finding global maxima that approaches 1, but is very slow.

Local Beam Search: Uses k randomly generated states, step, take k best successors, repeat. Accepts if the goal is found. Suffers from lack of diversity.

Genetic Algorithms: Successor state generated from 2 parents. Start with k randomly generated states (population). State represented as string over a finite alphabet. Evaluation function (fitness) to select parents. Flip bit after generating offspring according to a mutation rate. Culling to discard individuals below a given threshold.

## Constraint Satisfaction Problems

Formal representation language that allows for useful general-algorithms with more power than standard search algorithms.

### Definitions
- Variables {X1,...,Xn}
- Domains {D1,...,Dn}
- Constraints: Set that specifies allowable combination of values.

- Consistent Assignment: Assignment with no constraint violations.
- Complete Assignment: Every variable is assigned a value.
- Partial Assignment: Some variables are unassigned.
- Solution: Consistent, complete assignment.

- Unary Constraint: Involves 1 variable, e.g. USA != RED.
- Binary Constraint: Involves 2 variables, e.g. USA != UK.
- Higher-Order Constraints: Involves 3+ variables, e.g. X < Y < Z

- Discrete Variables: Finite domains - n variables, domain size d -> O(d^n) complete assignments. Infinite domains - integers/strings.
- Continuous Variables: Start/End times.

Real World CSPs: Scheduling.

Standard Search: Initial State {}, Assign value to unassigned variable and ensure it meets the constraints, fail if no legal assignments, accept if exists complete assignment. leafs=n!d^n

Backtracking Search: Abuses commutativity of CSPs. leafs=d^n, b=d. DFS with single variable assignments.

Most Constrained Variable: Choose variable with fewest legal values. (Priority)

Most Constraining Variable: Choose variable with the most constraints on remaining variables.

Least Constraining Value: Given a variable choose least constraining value. Leaves values available for other variables.

Forward Checking: Keeping track of remaining legal values for unassigned variables, terminates when any variable has no legal values.

Arc Consistency: X->Y is consistent iff every value x of X there is some allowed y.

Local Search for CSPs: Allow states with violations, operators reassign values. Randomly select any conflicted variable. Value selection by min-conflicts heuristic.

Iterative Min-Conflicts: While not at a solution, choose random conflicted variable, pick value with minimum conflicts.

## Adversarial Search

Game Tree Search: BFS for all gamestates.

### Minimax Search
MAX = self, MIN = opponent, maximise max and minimise min to simulate playing against a real player. 
- Use heuristic function to judge best/worst move. Goes from leaf to root. Complete (finite tree), Optimal (vs optimal opponent), t=O(b^m), s=O(bm) - DFS.
- Horizon Effect: Due to the inability to look at all possible states, significant effects might be just beyond our search space.
- Quiescence Search: Bad evaluation functions could lead to wild swings in evaluation (queen capture). Therefore we look for quiescence positions (stable) that resolve the uncertainties in the position. Can also search nonquiescence positions at a deeper ply until reaching a stable position.
- Single Extension: We allow for singular extensions for deeper ply for moves that are clearly better than all other moves in a given position.

### Alpha-Beta Pruning
Improved minimax that doesn't check branches that are not viable. 

Alpha: Lower Bound (MAX)

Beta: Upper Bound (MIN)

- Cut branches off if beta <= alpha.
- Prune because it doesn't matter what you will find in this branch, the value from the other side of the tree will be chosen anyways.
- Complete (finite tree), optimal (vs optimal opponent). 
- Pass alpha and beta down from parent to child, pass node value up to parent node and set as alpha/beta.
- Costs of Game Playing: Expansion - 50%, Evaluation - 40%, Search Control - 10%

## Planning
- Generate and Search over possible plans: sequence of actions to perform tasks
- Classical Planning: Deterministic, Finite, Static, Discrete.
- Progression Algorithm: Forward State-Space Search, considers effects of all possible actions in a given state.
- Regression Algorithm: Backwards State-Space Search, considers what must have been true in a previous state to get to the goal. Lower branching factor because only relevant actions are considered.
- Heuristics: Relaxed problem of removing preconditions from actions. Sub-goal independence - sums up cost to solve sub-problem independently.
- Partial Order Planning: Delays choices that don't need to be made during making of the plan. Utilised problem decomposition. Smaller search space than full order planning, and more flexible. Higher cost per node.

## Decision Trees
- Classification based on known features.
- Decision making based on known requirements/conditions.
- Takes series of inputs defining a situation and returns a binary decision/classification.
- A decision tree outlines the steps we take to decide the return value.
- We use observable attributes to predict the outcome.
- Core Idea: Choose the attribute which can reduce the uncertainty of the return value the most/provides the most information gain. We split the tree on this attribute, and repeat.
- Conditions: Add leaf node if all potential children in same node are identical. Avoid decision nodes that unconditionally gives the same output value.
- Properties: Human-readable and explainable. Efficient classification. Handles continuous and categorical variables.

## Nearest Neighbour Methods
- Classification based on similar instances to decide where a new instance belongs.
- Idea: Define geometric representation of the data points.
- Voronoi Partioning: Uses Euclidian distance to determine partitions.
- k-nearest neighbour (kNN): Decided with majority voting by looking at the k nearest neighbours. Distance is measured with Minkowski distance, p=2 Euclidian, p=1 Manhattan, Boolean attribute values = Hamming.
- Choosing k: k too small, noisy output. k too big, lose structural details. We choose multiple k and validate them to identify the best k value.
- Distance Metrics: Normalise data if using Euclidian space. (Rescale to be 0-1) This allows for data to not dominate each other when comparing disjoint data.
- Categorical Data: Don't use kNN because it is hard to rank. (e.g. Country)
- Aggregation Technique: Majority, Median, Average, Weighted Majority.

## Bayesian Reasoning

	P(a|b) = P(a) + P(b) - P(a∧b)
	P(b|a) = P(a∧b)/P(a)
	P(a∧b) = P(a|b)P(b)
	P(B) ∑a P(B and (A=a)) = ∑a P(B|a=a)P(A=a)

Bayes' Rule: `P(B|A) = P(A|B)P(B)/P(A)`

We can use it to calculate probability of hypothesis given evidence. Choose B = hypothesis, A = evidence.
- Inference: derive extra information/conclusions from observed data.
- Bayes' Net: Used in complex networks.
- Small Dataset: Joint Distribution (Truth Tables for Booleans). Problem: Doesn't show relationships between variables. Scales badly (brute force)
- Independence: Joint probability is product of their probabilities. P(A|B) = P(A*B)/P(B) = P(A)
- Building Bayesian Networks: Manually consulting domain experts for structure and probabilities. Probabilities also can come from data. (More common). Can also build them purely from data, by rewarding the net structure for matching data but penalising excess complexity.
- Bayesian Networks are directed acyclic graphs. Memory efficiency (not speed), and easy to interpret and explain for humans compared to joint distribution.
- Advantages: Captures uncertainty elegantly, can integrate prior knowledge, can be updated via Bayes' theorem as we obtain more data.
- Disadvantages: Using wrong prior can make it difficult to find correct answer, computational expensive (looks at all possible situations), not guaranteed correctness because it is probabilistic.

## Robotics
JIRA defines robots as devices with degrees of freedom that can be controlled: 6 tiers, manual, fixed sequence, variable sequence, playback, numerical control, intelligent.

# Readings

## Turing Test Positives
- Relatively simple and easy to understand, accessible to wide range of people.
- Allows for possibility of continuous improvement of the development of the intelligence.
- Clearly defined and easy to implement.
- Wide range of domain applications.
- Not biassed by how the machine looks, takes place through a terminal.

## Turing Test Negatives
- No quantifiable measure of intelligence.
- Subjective test based on a single human evaluator.
- Cannot test for some things such as visual intelligence due to the terminal interface.
- Only tests for human-like intelligence. Superhuman/subhuman intelligence may fail the Turing test.
- It's a behavioural test - Chinese room argument.
- Biassed in favour of machines that are able to mimic human behaviour.

## Searle's Chinese Room Argument
- Thought experiment to show that a machine can never truly understand language/think on its own.
- Scenario: Give question in Chinese, person in room looks up appropriate response in a rulebook, and outputs the answer. This person is assumed to know Chinese, when they are only following a set of rules.
- Analogous to a computer program that can respond to Chinese input, when it is just a bunch of rules that it follows.
- Argues ability to mainipulate symbols doesn't equate to understanding of those symbols, just like the person in the room doesn't understand Chinese.

## "Elephants Don't Play Chess" - Brooks
Traditionally artificial intelligence was thought of as applying formal rules to symbols, and he argues that it is limited because intelligence comes from the interaction of an agent and its environment, shaped from evolution and learning. It is also impossible to anticipate all interactions an agent may have with its environment.
