**Note:** This is not meant to be a comprehensive list of everything gone over
in class. Rather, it is just a set of personal notes where I jot down what I
perceive to be most important, including extra information wherever I see fit.
Hopefully this is of help to you.

# Famous Person

A person `p` is *famous* in a population `P` (consisting of `n` people) if:

* `p` does not know anyone else in `P`
* Everyone else in `P` knows `p`

**Question**: How can we find a famous person in the population, if one exists?

<details> <!-- Brute Force -->
<summary>Solution 1</summary>

#### Algorithm

For every person in the population:

* Ask `p` if they know anybody else in `P`. If they do, then `p` is not famous
  and we move on to the next person
* Ask if everybody else in `P` knows `p`. If any one does not, then `p` is not
  famous and we move on to the next person

#### Analysis

For the first person, we must ask `2n - 2` questions to determine whether or not
they are famous. For each subsequent person, this count drops by 2 since we have
eliminated the previous person. Adding this up for all `n` people yields that
this algorithm takes on the order of `n^2` steps.

</details>

<details> <!-- Clever Optimization -->
<summary>Solution 2</summary>

**Idea.** We waste a lot of efforts on asking questions to pairs of people that
have already asked each other questions. Can we do better?

#### Algorithm

While there is more than one person left in the population:

* Arbitrarily choose two people, say `p` and `p'`, and ask if `p` knows `p'`
* If `p` does know `p'`, then `p` cannot be famous, and so we remove them from
  our population
* If `p` does not know `p'`, then `p'` cannot be famous, and so we remove them
  from our population

Then go through one final time and ask if our final person knows anybody in the
original population, and vice versa to determine if they are famous.

#### Analysis

We eliminate the first `n - 1` people in `n - 1` questions, and then take `2n -
2` questions to ascertain whether the final person is famous or not. Hence this
algorithm runs in linear time.

</details>

<details> <!-- Notes -->
<summary>Notes</summary>

* There can be <i>at most one</i> famous person in the population
* Both solutions above use an <i>iterative</i> approach for solving this
  problem, by eliminating one candidate from the pool at a time

</details>

# Stable Matching

Suppose we have two groups, `G` and `G'`. Each member of `G` has an *absolute*
(strict, no ties) and *complete* (contains everybody from `G'`) ranking for
everybody in `G'`, and vice versa for every member of `G'`.

We call a matching *unstable* if there exists some `a, b` in `G` and `a', b'` in
`G'` such that `a` is matched with `a'` and `b` is matched with `b'`, but
either:

* `a` and `b'` would prefer to be matched with each other over their current
  partners
* `b` and `a'` would prefer to be matched with each other over their current
  partners

We define a *stable matching* to be a matching where there exists no unstable
matches.

**Question**: How can we find a stable matching for the two groups, if one
exists?

# Egg Problem

You are on a staircase with `n` steps, and have a certain quantity of eggs on
you. You may drop an egg on any of the steps, and it will either stay intact or
break.

**Question**: Given some number of eggs, what is the minimum number of drops
necessary to find the first step on which the eggs will start breaking?

<!-- TODO: Write up problem solution -->

# Interval Scheduling

Suppose you have `n` errands to run, where the start and end times of the `i`th
errand are `l_i` and `r_i`, respectively. You cannot perform multiple errands
simultaneously.

**Question**: What is the maximum number of non-overlapping errands that you
can do?

<details> <!-- Brute Force -->
<summary>Brute Force</summary>

Construct the power set of the set of intervals and iterate through the subsets.
Then find the maximum number of intervals in any subset with no overlapping
intervals. Since we generate the power set, the time complexity of this method
is `O(2^n)`.

</details>

# Majority Vote

Suppose we have `m` candidates in an election, labeled `c_1` through `c_m`, and
`n` votes for the `m` candidates. A *majority* occurs when somebody wins
*strictly more than* `n/2` votes.

**Question**: Has anyone won a majority of the votes?

<details> <!-- Brute Force -->

<summary>Solution 1</summary>

We iterate from candidate `1` all the way through candidate `m`, and count how
many votes that candidate has won (by iterating through the votes). If we find a
majority, then we can stop looking. The runtime is hence `O(mn)`.

</details>

<details> <!-- Boyer-Moore Voting Algorithm -->

<summary>Solution 2</summary>

If we remove two *distinct* votes, then the majority is maintained (if it
exists). This is because you are guaranteed to remove *at most* one
majority element.

After many such removals of pairs, if we are left with one value, then we
perform another pass to check if it is the majority candidate. To do this we
keep track of the current potential majority candidate, and how many more times
we have seen it than not.

#### Algorithm

Keep track of a variable `count = 0` and iterate through the list:

* If `count == 0` then overwrite the potential majority and increment count.
* Else if the current element is the same as the potential majority, increment
  `count`
* Otherwise the current element is different than the potential majority, so
  decrement `count`

If `count == 0` then there is no strict majority. Otherwise, take a final pass
through the votes and check if your potential majority is indeed your <i>actual</i> majority.

#### Analysis

As we only perform a constant number of computations for each vote, this
algorithm runs in `O(n)` time.

</details>

<details> <!-- Notes -->
<summary>Notes</summary>

* There is at most one majority candidate
* This problem is another example of *problem reduction*

</details>

## Graphs

* Adjacency matrix representation
  * The vertices represent the rows and columns, and if you have an edge between
    two vertices the corresponding element in the matrix is set to a 1,
    otherwise 0
  * Undirected graphs are *symmetric*, directed graphs not necessarily so
  * Space complexity `O(|V|^2)`
* Adjacency list representation
  * From each vertex, you have a linked list to all of its neighbors
  * Space complexity `O(|E| + |V|)`

### Breadth First Search

We traverse each edge twice, and every vertex once. Hence the time complexity is
`O(|E| + |V|)`.

* If the graph is connected, `O(|E|)` ≥ `O(|V|)` and we may simplify it to
  `O(|E|)`
* If the graph is disconnected, we modify the algorithm to "jump" when we run
  out of nodes in the current component. We stop only when we have successfully
  marked all nodes
* Can be used to generate a minimal spanning tree (BFS Tree)
  * The edges in this tree are called *primary edges*, with all other edges
    being *secondary*

### Depth First Search

Again, we traverse each edge twice, and every vertex once. Hence the time
complexity is `O(|E| + |V|)`.

### Shortest Distance Algorithm

Given two vertices `u` and `v` in a graph, find the *distance* (length of a
shortest path) between them.

<details>
<summary>Solution 1</summary>

The shortest path from `u` to `v` is the level of `v` with respect to `u`'s
BFS tree.

</details>

## Topological Sort

Consider a directed acyclic graph `G`. The *precedence relationship* between
nodes `u` and `v` states that if there is a path from `u` to `v`, then `u` must
come before `v`.

**Question**: Find an ordering of the nodes that does not violate any
*precedence relationships*.

### Computing In/Outdegrees

* We can find the outdegree of each node by traversing its linked list of
  neighbors, and so we can compute the indegree of each node as well
* We can traverse all the edges in `O(|V|^2)` time, and find the in/outdegrees
  of all the nodes
  * We can make this better by restricting it to `O(|E| + |V|)`, as on average it is
    better than `O(|V|^2)`
  * We need the `O(|V|)` because the graph might not be connected
* A *source* of a graph is a node with indegree 0, so the above will find our
  sources after another pass through all the nodes
  * **Bonus**: Show that there exists at least once source

<details>
<summary>Solution 1</summary>

We maintain a list of all the sources in our DAG.

* After removing a source node, you output the node and remove any outgoing
  edges
  * The graph remains a DAG, and now we update the indegrees of the other nodes
    and add our new sources to our list

It takes `O(|E| + |V|)` time to both find all in/outdegrees, as well as traverse
through the graph and output all the nodes in topological order.

</details>

### DAG Grouping

Let `G` be a DAG, and `k` the maximum length of any path in `G`. Partition the
vertices into `k + 1` groups such that for each pair of vertices in the same
group, there are no paths.

<details>
<summary>Solution 1</summary>

Topological sort, but take out <i>all</i> sources at once instead of just some
of them, and put them into their own group.

</details>

**Bonus**: Given a DAG, what is the length of the longest path in the DAG?

## Graph Reversal

Given a directed graph `G`, reverse the direction of all of the edges.

<details>
<summary>Solutions</summary>

* Transpose the adjacency matrix
* Traverse the linked lists and create a new set of linked lists

The former runs in `O(|V|^2)` time, and the latter runs in `O(|E| + |V|)` time.

</details>

## Strongly and Weakly Connected Graphs

We call directed graph `G` *strongly connected* if for all pairs of vertices
`(u, v)`, if there exists a path from `u` to `v`, then there is a path from `v`
to `u`. We say `u` and `v` are *mutually reachable*.

* If `u` and `v` are mutually reachable, and `v` and `w` are mutually reachable,
  then `u` and `w` are mutually reachable

A *strong component* of a node `u` is the set of all vertices `x` in `v` such
that `x` and `u` are mutually reachable.

* For all `u, v` in `v`, we have that the strong components of `u` and `v` are
  the same, or they are disjoint.
  * If `u` and `v` are mutually reachable, then for any node `x` in the strong
    component of `u`, we have `x` and `v` are mutually reachable. Hence the
    strong components of `u` and `v` are the same.
  * If they are not mutually reachable, then we show that the intersection of
    the strong components is empty. Suppose towards a contradiction that there
    exists some node `x` in the intersection. Then `u` and `v` are mutually
    reachable, a contradiction.

## Determine Whether a Graph is Strongly Connected

Given a directed graph `G`, determine whether or not `G` is strongly connected.

<details>
<summary>Solution</summary>

We pick some node `s` and BFS starting from `s`. Then we
perform a graph reversal and try BFS from the same node `s`. If the
two graphs are the same, then it is a strongly connected component.

* If it is strongly connected, then all pairs of nodes are mutually reachable,
  so for all nodes `s`, we have that `s` is mutually reachable with every node.
* If there exists some `s` for which `s` is mutually reachable with every node
  `u`, then for all other nodes `v`, we have `u` is mutually reachable with `v`
  (by transitivity).

</details>

## Shortest Path in a Weighted Graph (Dijkstra's)

Consider a weighted graph `G` with positive weights, and some starting node `s`.
For some other node `v`, find the length of the shortest path from `s` to `v`.

<details>
<summary>Solution 1</summary>

#### Algorithm

* Let `S` be the set of explored nodes
* Set the distance from `s` to itself to be 0, and the distance to all other
  nodes to be infinite.
* while `S` and `V` are different:
  * Iterate through all neighbors of `s` (call this neighbor `v`)
  * If `v` not in `S`, then find such a `v` that results in the minimum distance
    from the current node
  * Add such a `v` to `S`, and update `v`'s distance
  * Update `v`'s predecessor to be `u`

#### Proof

* Consider `S`
  * For all `u` in `S`, show that `P_u` is the shortest path from `s`
* We induct on the size of `S = |S| = k`
  * When `k` is 1, this clearly holds
  * Let's this holds for some `k`, and we wish to add another node `v` to `S`
  * Since this path has to come out of `S` at some point, all other paths will be
    suboptimal compared to the shortest paths coming out of `S`
    * Suppose towards a contradiction that our path is suboptimal
    * Since all other paths would have had to come out of `S` at some point
      (before reaching `v`), our algorithm would have also considered them
    * As our algorithm did not choose any of the other paths, our path must be
      the shortest

In the worst case, we process each edge *at most* once. When we process each
edge, we perform a deletion and insertion to our heap to "update" the distances
there. Hence the runtime complexity is `O(|E| log |V|)` or `O(|E| log |E|)`.

An alternative method is for each node, update every other node and find the
minimum that way. Hence the runtime would be `O(|V|^2)`. This is better when the
graph is dense, but not when it is sparse.

</details>

## Parenthesis Theorem (For DFS)

Motivation: When you consider a valid string of parentheses, we have close the
most recent set of parentheses before closing older sets. For example, `([])` is
a valid set but `([)]` is not.

Suppose `G` is a graph and `u, v` are nodes in `G`.

* Case 1: Then either `u` was processed before `v` was seen or vice versa.
* Case 2: Then `u` was seen, `v` was seen, `v` was processed, `u` was processed
  * `v` is a descendant of `u`

Relating back to the motivation, the above theorem just states that for a given
DFS algorithm, one node has to have been processed before the other was seen
(`()[]`), or we have case 2 in the above statement (`([])`).

## White Path Theorem

Consider a DFS forest of `G` (directed or not)

* `v` is a descendant of `u` if and only if at the time that `v` is discovered,
  `u` has already been discovered

## Kosaraju's Algorithm

* Make a stack
* Perform DFS at any node and push nodes onto the stack (in post-order)
* Compute the graph reversal
* Continuously pop off the stack
  * Perform a DFS using the top of the stack and the graph reversal
  * Each set of nodes that make up these new DFS trees is a strongly connected
    component

## Spanning Trees

A *spanning tree* of connected graph `G` is a tree that contains all of the
nodes in `G`. A *minimal spanning tree (MST)* of connected weighted graph `G` is
the spanning tree that has the minimum total weight. *Cut edges* are edges that
go from one partition to another.

**Bonus:** Show that if the weights of a graph are unique, the MST is unique.

### Finding a Minimal Spanning Tree (Prim's)

Given a weighted, connected graph `G`, find a MST for `G`.

<details>
<summary>Solution 1</summary>

We arbitrarily partition the nodes of `G` into two subsets. Then we claim that
the minimum cut edge is a part of the MST.

* Suppose towards a contradiction that such an edge is *not* a part of the MST
* Then if we have the following cases:
  * If there is no edge between these two partitions, then the graph is
    disconnected, a contradiction
  * If this minimal edge is not in the MST, we may produce a spanning tree with
    smaller weight by replacing the edge in the MST with our minimal edge, a
    contradiction

If we perform this partitioning `|V| - 1` times and find `|V| - 1` edges, then we
have our MST. In particular, we process each node and edge exactly once.
However, every time we process an edge, we must consider the minimal weight of
all remaining edges connecting the groups (heap!), so this algorithm runs in
`O(|V| + |E| log |E|)` time. We may alternatively use a list and linear search,
yielding a runtime of `O(|V|^2)`

</details>

<details>
<summary>Alternate Proof</summary>

Consider a tree with `|E|` edges and `|V|` vertices. If we arbitrarily add a new
edge to the tree, we will have `|V|` edges and *exactly* one cycle. If we then
remove any edge from this cycle, the graph reverts to a tree (not necessarily
the original).

We claim that a MST always contains the minimal cut edge between the two groups
of a partition of `|V|`.

* Suppose towards a contradiction that the minimal spanning tree *does not*
  contain the minimal edge
  * We add the minimal edge to our minimal spanning tree, creating a cycle
  * We may then remove any of the other edges in the cycle to obtain a spanning
    tree with smaller weight than our MST, a contradiction

</details>

### Finding a Minimal Spanning Tree (Kruskal's)

Given a weighted, connected graph `G`, find a MST for `G`.

<details>
<summary>Solution 1</summary>

* Sort our `|E|` edges by weight
* Choose the edge with smallest weight and put it into our MST
  * As it has the smallest weight, it must be the minimal cut edge for any
    bipartition
* For our second edge, we create a bipartition by putting one end vertex in one
  group and all other vertices in the other group
* For every successive edge, we only add it to our MST if we can choose a
  partition such that *it is the minimal cut edge for that partition*
  * If adding this new edge creates a cycle, we can't add that edge to our MST,
    as we wou/ld have another edge with smaller weight in our MST already
    * We determine whether or not we create cycles using union-find
    * This can be performed in `O(log |E|)` time

Our sorting takes `O(|E| log |E|)` time, and we iterate through all the edges
again, performing union-find, so our algorithm overall takes `O(|E| log |E|)`.

#### Union-Find

We have a bunch of subsets of elements. Union-find will return whether or not
two elements belong to the same subset. We also want to be able to combine
various subsets.

* We start off with `|V|` separate subsets
* The "union" operation: We take a pointer from the root and point it to another
  group
  * This operation takes constant time
* The "find" operation: Follow both elements to their respective roots and
  return whether or not they are the same element
  * This operation takes time relative to the depths of both elements' rooted
    trees (we must perform our "unions" in a *efficient* way)
    * We *always* take the shorter rooted tree and point its root to the height
      of the taller rooted tree
    * Note that the height of the new tree is always going to be *at most* the
      maximum height of the two trees
    * If the heights of the two trees are the same, then the height of the new
      rooted tree is one more than the height of the original rooted trees
    * However, we have also increased the number of nodes, so we still have `h <
      log n`, where `h` is the height of our rooted tree and `n` is the number
      of nodes in the rooted tree
  * Thus the union operation takes `O(log |E|)` time

</details>

## Divide and Conquer

### Sorting

Given a list of `n` numbers, sort it to be in non-decreasing order.

<details>
<summary>Solution 1</summary>

#### Merge Sort Algorithm

* We first split our array into roughly two equal subarrays
* Recursively split each subarray into "left" and "right" subarrays
* At the very end of it all, we have `n` subarrays of size 1, which are already
  sorted
* We then take each set of "left" and "right" subarrays and *merge* them:
  * Take two pointers to the first elements of the "left" and "right" subarrays
  * Iterate through both arrays:
    * If the left pointer's value is smaller, add it's value to a list and
      increment the pointer
    * Otherwise, add the right pointer's value to a list and increment that
      pointer
* The resulting list from merging the two subarrays is also sorted

#### Analysis

When merging two subarrays of size `x` and `y`, we have that it takes `O(x + y)`
time, since each element in the merged array is a result of one operation
(comparison and pointer advancement).

As each "set" of merges iterates through all the elements in our array, it
runs in `O(n)` time. Furthermore, we perform this merge `O(log n)` times,
because we perform it as many times as we are able to split our array into left
and right subarrays. Hence the runtime complexity is `O(n log n)`.

Formal proof: Let the runtime be `T(n) = 2T(n/2) + cn`, for some constant `c`.

</details>

### List Inversions

We say that a pair `x, y` is *inverted* in a list if the index of `x` is smaller
than the index of `y`, but `x > y`. Find the number of inversions in an array of `n` values.

<details>
<summary>Solution 1</summary>

#### Algorithm

This algorithm is a simple modification of merge sort.

* Consider that during the merge step, we compare two pointers from the left and
  right subarrays
  * If the number in the left subarray is larger than the number in the right
    subarray, then we add the *remaining* number of elements in the left
    subarray to our partial sum
  * Otherwise, proceed with merge sort as usual

Note that at each recursive step, we already have the number of inversions in
the left and right subarrays, so all that remains is the number of inversions
with one element in the left subarray and one in the right subarray.

Since this only adds a constant number of operations to merge sort, it still
runs in `O(n log n)`

</details>

### Closest Pair Problem

Given a set of `n` points in the xy-plane, find the pair of points that has the
shortest distance between them. You may use any distance metric (L1 norm, L2
norm, etc.).

<details>
<summary>Solution 1</summary>

* Sort the points by their x-coordinate first
* Separately, sort the points by their y-coordinate and store that in a separate
  list
* Partition the points into two groups based on their x-coordinate
* The minimum distance pair is the minimum of the minimum distance pairs of both
  groups, or a new pair with one point in the left group and the other point in
  the other group
* Let the minimum distance for the right group be `d_2`
  * If we create a "grid" where each square has length `d_2/2`, we have that we
    only need to consider *at most* 8 squares for each point in the left
    subgroup
    * This is because considering anything other than the 8 closest squares
      would be at a distance more than `d_2` away
  * Furthermore, observe that each square of our grid contains *at most* 1
    points, so we have a constant number of points to consider in the right
    group, for every point in the left group
    * We can then use our points that are sorted by y-coordinate to find the
      8 closest neighbors, and then compare those to our point in the left group

Hence we have that at each "merge" step, we perform a linear number of
computations (constant for each point in the left subgroup), so the runtime
complexity is also `O(n log n)`.

</details>

## Dynamic Programming (DP)

Given a problem space, we partition it into subproblems and get the optimal
answer to each subproblem. We then combine the solutions to the subproblems to
get the solution to the overall problem.

### Weighted Interval Scheduling

Given a list of `n` weighted intervals, maximize the sums of the weights of
non-overlapping intervals.

We assume that the start and endpoints are *distinct*.

<details>
<summary>Solution 1</summary>

#### Algorithm

* Sort the intervals by ending time
* Iterate through the sorted intervals list
* For each interval in the list
  * Compute the highest weight sum of the set of intervals that ends at (but
    does not necessarily include) the current interval
    * Binary search for the latest-ending interval that does not overlap with
      the current interval
    * Add that interval's maximal sum to the current interval's weight, and
      compare it to the previous interval's maximal sum
      * Set the current interval's maximal sum to whichever of the two sums is
        larger
* Return the final interval's maximal sum

#### Analysis

We first sort the intervals, and then for each interval we perform a binary
search, so the runtime is `O(n log n)`.

If we assume that the list is already sorted, and that the lookup in the DP
array is constant, then the runtime complexity is `O(n)`.

#### Idea

* The general idea is that at each step, the current interval is either *in* the
  optimal weighted sum up until that point, or it *is not*
* Because of this, we can just find the maximal sum that doesn't overlap with
  the current interval, add the current weight, and compare it to the most
  recent maximal sum

</details>

## Knapsack Problem

We are given a set of `n` items, each with size `s_i` and value `v_i`. You have
a knapsack with some maximum size `S`.

**Question**: Find the maximal value of items that will fit in the knapsack.

**Related**: What if you can only have *at most* one of each kind of item? What
if for all items `i`, you can have *at most* `K_i` of that kind of item?

<details>
<summary>Solution 1</summary>

The Crux of the Problem:

* Let the current item that we are considering be item `i`, and the current
  knapsack size be `j`
* Then we have two options:
  * Include item `i` in our knapsack
    * If this is the case, we have a value of `dp[i][j - s_i] + v_i`
  * Don't include item `i` in our knapsack
    * If this is the case, we have a value of `dp[i - 1][j]`
* Hence we take the maximum of these two values, yielding `max(dp[i][j - S_i] +
  v_i, dp[i - 1][j])`

Since we only perform a constant number of operations for each element in our
`dp` array, we have that our algorithm runs in a runtime complexity of `O(nS)`.

**Note**: This algorithm is *not* a polynomial algorithm because it is not
polynomial *with respect to the input parameters*. In particular, if `S` is a
polynomial in terms of `n`, then we have a polynomial-time algorithm. If `S` is
exponential in terms of `n`, then we don't. We call these kinds of algorithms
*pseudo-polynomial*.

</details>

# Longest Common Subsequence

From a given alphabet we have the strings `L` and `R`. A *subsequence* is a
string formed by deleting an arbitrary number/count from another string. A
*common subsequence* of strings `L` and `R` is a string that is a subsequence of
both `L` and `R`.

**Question**: Find the length of the longest common *subsequence* to `L` and
`R`.

**Related**: Find the length of the longest common *substring* to `L` and `R`.

<details>
<summary>Solution 1</summary>

* Let `dp[i][j]` denote the length of the longest common subsequence for
  substrings of `L` and `R` ending at indices `i, j`, respectively
* If `L[i] == R[j]`:
  * `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1])`
* `dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])`

The runtime complexity of this algorithm is `O(mn)`, as we perform a constant
number of computations for each possible character pair between the strings.

</details>

## Generalized Dijkstra's (not positive)

Consider a weighted graph `G` with potentially negative edges and no negative
cycles, and some starting node `s`.

**Question**: For some other node `v`, find the total weight of the shortest
path from `s` to `v`.

<details>
<summary>Solution 1</summary>

* We know that the maximum length of the smallest weighted path is at most
  `|V| - 1`
  * If we had `|V|` or more edges, then at least one vertex would have been in
    our path twice, and we would have a cycle
  * Since our cycle has to have positive weight, we have that removing the cycle
    decreases the overall path length, and so our smallest weight path does not
    have the smallest weight
* We know how to find the distance to all other nodes of path length 1
* Given the smallest weighted paths of lengths 1 to `k - 1`, we wish to find the
  smallest weighted path of length `k`
  * We iterate through all terminating points of path length `k - 1`, and
    exhaustively compute the shortest paths of length `k`

Since in the worst case we traverse every node and edge when updating our path
length, and we iterate the length from 1 to `|V| - 1`, our worst-case time
complexity is `O(|V| • (|V| + |E|))` or `O(|V|^3)`.

</details>

## Curve-Fitting

Consider a set of points with another set of lines. We define an error function
by `E = e + λl` (which could be different), where `e` is the net distance to the
line segments from the points (via some norm), `λ` is some constant, and `l` is
the number of lines.

**Question**: Find an optimal set of line segments to minimize the error.

<details>
<summary>Solution 1</summary>

* We start by sorting the points by x-coordinate
* We try to find the best fit line for `i + 1` points, based on whether or not
  we know the best fit line for `i` points
* We know that the last line segment must either cover the last point, or we add
  a new line segment that covers it
  * Thus for every iteration, we compute `i` best fit lines that minimize error
    through the last 1,2,... points

</details>

## Network Design

Consider a connected directed graph with one vertex designated as the *source*
and another designated as a *sink*. Each edge has a *capacity* `c_i`. We call
such a graph a *network*. The elements that we send along each edge is called
*flow*, and is denoted `f_i`. Hence we have that `0 ≤ f_i ≤ c_i` for all `i`.

<!-- Definition: Conservation of Flow -->

> **Conservation of Flow:** The net flow going into a vertex must be equal to
> the net flow leaving a vertex, except for potentially the source/sink.

### Maximum Flow

Consider a network `N` with integral capacities `c_i`. A network that contains
reversed edges with capacities equivalent to the flow on their normal
counterparts is called a *residual network* or *augmented network*.

**Question**: Among all legal flows, find the one with maximum total flow
leaving the source.

<details>
<summary>Solution 1</summary>

#### Algorithm

* Start with an outgoing flow of zero
* While there exists a path from the source to the sink (with strictly positive
  capacities)
  * Increment the outgoing flow
  * For every edge in the path from source to sink
    * Decrement that edge's capacity
    * Increment that edge's reversal's capacity
* Return the number of outgoing flow

#### Proof

We bipartition the nodes in our network into a set containing the source, and a
set containing the sink. Then we have a series of cut edges across the
bipartition, say with capacities `c_1,...,c_m`. We know that to send flow from
the source to sink, it must necessarily pass through one of those cut edges. We
define the capacity of a cut to be the sum of `c_1,...,c_m`. Observe that the
maximum flow is *at most* the capacity of the minimal cut.

We claim that this flow we return is maximal.

Suppose towards a contradiction that the maximum flow is unequal to the value
returned by the algorithm. First, we assume that the maximum flow is less than
the value returned by the algorithm. As the algorithm has already found a flow
with larger flow than the maximum, we have a contradiction. Suppose that the
maximum flow is strictly larger than the value returned by the algorithm.

</details>
