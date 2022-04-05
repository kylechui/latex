# Famous Person

A person `p` is *famous* in a population `P` (consisting of `n` people) if:

* `p` does not know anyone else in `P`
* Everyone else in `P` knows `p`

**Question**: How can we find a famous person in the population, if one exists?

<details>
<summary>Brute Force</summary>
<h4>Algorithm</h4>

For every person in the population:

<ul>
  <li>
    Ask <code>p</code> if they know anybody else in <code>P</code>. If they do, then <code>p</code> is not famous
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

<details>
<summary>Clever Optimization</summary>
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

<details>
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

**Question:** Given some number of eggs, what is the minimum number of drops
necessary to find the first step on which the eggs will start breaking?
