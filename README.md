# toilet-paper-pieces
How many pieces is in roll of toilete paper? Let's find it out with Python.

I wanted to know, how much pieces of toilet paper is in roll based on several parameters.

```
ri = r_inner
ro = r_outer

      *******
   *           *
  *     ***     *
 *    *ri   * ro *
 *   *<--o------>*
 *    *     *    *
  *     ***     *
   *           *
      *******
```

`r_inner` is diameter of inner circle (roll base) \
`r_outer` is diameter of outer circle (roll base with pieces)

![\sum_{i=0}^{m} a_i = \frac{2 \cdot \pi \cdot (r_{inner} + (n + 1))}{l+(1-a_{n-1})}](sum-eq.png)
