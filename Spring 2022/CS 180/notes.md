# Famous Person

A person `p` is *famous* in a population `P` (consisting of `n` people) if:

* `p` does not know anyone else in `P`
* Everyone else in `P` knows `p`

**Question**: How can we find a famous person in the population, if one exists?

<details> <!-- Brute Force -->

<summary>Solution 1</summary>

<h4>Algorithm</h4>

For every person in the population:

<ul>
  <li>
    Ask <code>p</code> if they know anybody else in <code>P</code>. If they do,
    then <code>p</code> is not famous
    and we move on to the next person
  </li>
  <li>
    Ask if everybody else in <code>P</code> knows <code>p</code>. If any one
    does not, then <code>p</code> is not famous and we move on to the next
    person
  </li>
</ul>

<h4>Analysis</h4>

For the first person, we must ask `2n - 2` questions to determine whether or not
they are famous. For each subsequent person, this count drops by 2 since we have
eliminated the previous person. Adding this up for all `n` people yields that
this algorithm takes on the order of `n^2` steps.

</details>

<details> <!-- Clever Optimization -->

<summary>Solution 2</summary>

<b>Idea.</b> We waste a lot of efforts on asking questions to pairs of people that
have already asked each other questions. Can we do better?

<h4>Algorithm</h4>

While there is more than one person left in the population:

<ul>
  <li>
    Arbitrarily choose two people, say <code>p</code> and <code>p'</code>, and
    ask if <code>p</code> knows <code>p'</code>
  </li>
  <ul>
    <li>
      If <code>p</code> does know <code>p'</code>, then <code>p</code> cannot be famous, and so we remove them
      from our population
    </li>
    <li>
      If <code>p</code> does not know <code>p'</code>, then <code>p'</code> cannot be famous, and so we remove
      them from our population
    </li>
  </ul>
</ul>
Then go through one final time and ask if our final person knows anybody in the
original population, and vice versa to determine if they are famous.

<h4>Analysis</h4>

We eliminate the first `n - 1` people in `n - 1` questions, and then take `2n -
2` questions to ascertain whether the final person is famous or not. Hence this
algorithm runs in linear time.

</details>

<details> <!-- Notes -->
<summary>Notes</summary>
<ul>
  <li>
    There can be <i>at most one</i> famous person in the population
  </li>
  <li>
    Both solutions above use an <i>iterative</i> approach for solving this
    problem, by eliminating one candidate from the pool at a time
  </li>
</ul>
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
majority, then we can stop looking. The runtime is hence `O(m*n)`.

</details>

<details> <!-- Boyer-Moore Voting Algorithm -->

<summary>Solution 2</summary>

If we remove two *distinct* votes, then the majority is maintained (if it
exists). This is because you are guaranteed to remove <i>at most</i> one
majority element.

After many such removals of pairs, if we are left with one value, then we
perform another pass to check if it is the majority candidate. To do this we
keep track of the current potential majority candidate, and how many more times
we have seen it than not.

<h4>Algorithm</h4>

Keep track of a variable `count = 0` and iterate through the list:

<ul>
  <li>
    If <code>count == 0</code> then overwrite the potential majority and
    increment count.
  </li>
  <li>
    Else if the current element is the same as the potential majority,
    increment <code>count</code>
  </li>
  <li>
    Otherwise the current element is different than the potential majority, so
    decrement <code>count</code>
  </li>
</ul>

If `count == 0` then there is no strict majority. Otherwise, take a final pass
through the votes and check if your potential majority is indeed your <i>actual</i> majority.

<h4>Analysis</h4>

As we only perform a constant number of computations for each vote, this
algorithm runs in `O(n)` time.

</details>

<details> <!-- Notes -->
<summary>Notes</summary>

<ul>
  <li>
    There is at most one majority candidate
  </li>
  <li>
    This problem is another example of <i>problem reduction</i>
  </li>
</ul>

</details>

## Graphs

* Adjacency matrix representation
  * The vertices represent the rows and columns, and if you have an edge between
    two vertices the corresponding element in the matrix is set to a 1,
    otherwise 0
  * Undirected graphs are *symmetric*, directed graphs not necessarily so
  * Space complexity `O(n^2)`
* Adjacency list representation
  * From each vertex, you have a linked list to all of its neighbors
  * Space complexity `O(e + n)`

### Breadth First Search

We traverse each edge twice, and every vertex once. Hence the time complexity is
`O(e + n)`.

* If the graph is connected, `O(e)` ≥ `O(n)` and we may simplify it to
  `O(e)`
* If the graph is disconnected, we modify the algorithm to "jump" when we run
  out of nodes in the current component. We stop only when we have successfully
  marked all nodes
* Can be used to generate a minimal spanning tree (BFS Tree)
  * The edges in this tree are called *primary edges*, with all other edges
    being *secondary*

### Depth First Search

Again, we traverse each edge twice, and every vertex once. Hence the time
complexity is `O(e + n)`.

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
* We can traverse all the edges in `O(n^2)` time, and find the in/outdegrees of
  all the nodes
  * We can make this better by restricting it to `O(e + n)`, as on average it is
    better than `O(n^2)`
  * We need the `O(n)` because the graph might not be connected
* A *source* of a graph is a node with indegree 0, so the above will find our
  sources after another pass through all the nodes
  * **Bonus**: Show that there exists at least once source

<details>
<summary>Solution 1</summary>

We maintain a list of all the sources in our DAG.

<ul>
  <li>
    After removing a source node, you output the node and remove any outgoing
    edges
  </li>
  <ul>
    <li>
      The graph remains a DAG, and now we update the indegrees of the other
      nodes and add our new sources to our list
    </li>
  </ul>
</ul>

It takes `O(e + n)` time to both find all in/outdegrees, as well as traverse
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

The former runs in `O(n^2)` time, and the latter runs in `O(e + n)` time.

</details>

## Strongly and Weakly Connected Graphs

We call directed graph `G` *strongly connected* if for all pairs of vertices
`(u, v)`, if there exists a path from `u` to `v`, then there is a path from `v`
to `u`. We say `u` and `v` are *mutually reachable*.

* If `u` and `v` are mutually reachable, and `v` and `w` are mutually reachable,
  then `u` and `w` are mutually reachable

A *strong component* of a node `u` is the set of all vertices `x` in `V` such
that `x` and `u` are mutually reachable.

* For all `u, v` in `V`, we have that the strong components of `u` and `v` are
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
We pick some node <code>s</code> and BFS starting from <code>s</code>. Then we
perform a graph reversal and try BFS from the same node <code>s</code>. If the
two graphs are the same, then it is a strongly connected component.

* If it is strongly connected, then all pairs of nodes are mutually reachable,
  so for all nodes `s`, we have that `s` is mutually reachable with every node.
* If there exists some `s` for which `s` is mutually reachable with every node
  `u`, then for all other nodes `v`, we have `u` is mutually reachable with `v`.

</details>

## Shortest Path in a Weighted Graph

Consider a weighted graph `G` with positive weights, and some starting node `s`.
For some other node `v`, find the length of the shortest path from `s` to `v`.

<details>
<summary>Solution 1</summary>

* Let `S` be the set of explored nodes
* Set the distance from `s` to itself to be 0, and the distance to all other
  nodes to be infinite.
* while `S` and `V` are different:
  * Iterate through all neighbors of `s` (call this neighbor `v`)
  * If `v` not in `S`, then find such a `v` that results in the minimum distance
    from the current node
  * Add such a `v` to `S`, and update `v`'s distance
  * Update `v`'s predecessor to be `u`

Proof (Stay ahead):

* Consider S
  * For all u in S, show that P\_u is the shortest path from s
* We induct on the size of S = |S| = k
  * When k is 1, this clearly holds
  * Let's this holds for some k, and we wish to add another node v to S
  * Since this path has to come out of S at some point, all other paths will be
    suboptimal compared to the shortest paths coming out of S
    * STAC; If our path is not optimal, our algorithm would have picked the
      other path (as it has already been considered)

This algorithm runs in `O((e + n) * log(n))` (linear + heap).

</details>

## Parenthesis Theorem (For DFS)

Suppose `G` is a graph and `u, v` are nodes in `G`.

* Case 1: Then either `u` was processed before `v` was seen or vice versa.
* Case 2: Then `u` was seen, `v` was seen, `v` was processed, `u` was processed
  * `v` is a descendant of `u`

## White Path Theorem

Consider a DFS forest of `G` (directed or not)

* `v` is a descendant of `u` if and only if at the time that `v` is discovered,
  `u` has already been discovered

## Kasaraju's Algorithm

* Make a stack
* Perform DFS at any node
* Compute the graph reversal
* Pop off the stack and compute more DFS's with the graph reversals
