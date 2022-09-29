# Data Representation and Manipulation

* How characters and numbers are represented in a typical computer

## Characters

* Common characters are typically represented using ASCII, which uses 7 bits to
  represent 128 different characters

## Numbers

* We can represent by a digit-vector (just writing the number in base $k$)
* We call this base the *radix*
* For example, $(121)\_3 = 16$

### Unsigned Binary Numbers

* With $k$ bits, we can represent up to (but excluding) $2^k$

### Signed Binary Numbers

* We could use the first bit to denote the sign of the number, but then we run
  into the issue of having both a positive and negative zero
* Instead we use two's complement, where the MSB represents a negative power of
  two
* To invert a two's complement number, we compute $-x = \bar{x} + 1$
* In two's complement, to extend a number, all we need to do is prepend copies
  of the old MSB
* Addition for two two's complement numbers is done exactly how you expect
* To subtract, just negate and add
* Sometimes we can have integer overflow

## Switching Expressions

* Symbols 0 and 1 are SEs
* A symbol representing a binary variable is a SE
* If $A$ and $B$ are SEs, then
  * Complement (NOT): $(A)'$ is a SE
  * Sum (OR): $A + B$ is a SE
  * Product (AND): $A\cdot B$ is a SE
