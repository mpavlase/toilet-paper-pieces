# toilet-paper-pieces
How many pieces is in roll of toilete paper? Let's find it out with Python.

I wanted to know, how much pieces of toilet paper is in roll based on several parameters. The problem is that diameter is increasing by every layer of previous pieces.

## Roll
Let's demonstrate it in this SSD [1]
```
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

`r_inner` (`ri` in drawing) is diameter of inner circle (roll base)

`r_outer` (`ro` in drawing) is diameter of outer circle (roll base with pieces)

----
[1] Simply Stupid Drawing

## Piece (better to say *sheet*)

The sheet is a rectangle. We are interested in just one dimension - length, that is involved in total diameter.

```
+---------+    A
|         |    |
|         |    | we don't care about this
|         |    |
+---------+    V

<--------->
   length
```

# Example

I took a roll (not opened yet), grab my caliper and got these input values:

\begin{equation*}
d_{inner} = 40\,mm
\end{equation*}

\begin{equation*}
d_{outer} = 105\,mm
\end{equation*}

\begin{equation*}
thikness = 0.25\,mm
\end{equation*}

\begin{equation*}
length = 130\,mm
\end{equation*}

---
($thikness$ is just rough value, because paper is *fluffy* and that's why difficult to measure precisely. I measured it as average of folded 10 layers.)

A sheet doesn't fit to roll base diameter exactly (it's smaller from beginning), so we sould evalate just a part of layer, that is wound to the same diameter. Let's call it as $z$ with index of current iteration.

$a_0$ is number of sheets / current diameter

\begin{equation*}
a_0 = \frac{perimeter_0}{length}
\end{equation*}
after substition:
\begin{equation*}
a_0 = \frac{2 \cdot \pi \cdot r_{inner}}{length}
\end{equation*}

Leftover $z$ is:
\begin{equation*}
z_0 = 1 - a_0
\end{equation*}

Great, we have first iteration. How it will look like another?

\begin{equation*}
a_1 = \frac{2 \cdot \pi \cdot (r_{inner} + thikness)}{length + z_0}
\end{equation*}
after $z_0$ substition:
\begin{equation*}
a_1 = \frac{2 \cdot \pi \cdot (r_{inner} + thikness)}{length + (1 - a_0)}
\end{equation*}

From that quation we can find a pattern that can be described as sum expression.
\begin{equation*}
\sum_{i=0}^{m} a_i = \frac
{2 \cdot \pi \cdot (r_{inner} + (n + 1) \cdot thikness)}
{l+(1-a_{n-1})}
\end{equation*}
... where $m$ is number of toilete paper layers.

# Final evaluation: *"Python, help me!"*


```python
from math import pi

# all numbers are in mm

d_inner = 40
d_outer = 105
thikness = 0.25
piece_length = 138

r_inner = d_inner / 2
r_outer = d_outer / 2

final_pieces = 2 * pi * r_outer / piece_length

def pieces_per_round(number_of_turns, previous_leftover_length):
    return (2 * pi * (r_inner + number_of_turns * thikness)) / (piece_length + previous_leftover_length)

def leftover_on_round(pieces):
    return 1 - pieces

def is_last_round(pieces):
    return pieces <= final_pieces

i = 0
previous_leftover_length = 0
sum_pieces = 0

print(f'Expected end about {final_pieces:1.3f} pieces/round')

while True:
    pieces = pieces_per_round(i, previous_leftover_length)
    leftover = leftover_on_round(pieces)
    #print(f'Round {i:3}: {pieces:1.3f} pieces/round with {leftover:1.3f} leftover')
    
    previous_leftover_length = leftover
    sum_pieces += pieces
    
    if not is_last_round(pieces):
        break
    
    i += 1

print(f'Round {i:3}: {pieces:1.3f} pieces/round with {leftover:1.3f} leftover')
print(f'Whole roll contains about {sum_pieces:1.0f} pieces.')
```

    Expected end about 2.390 pieces/round
    Round 128: 2.391 pieces/round with -1.391 leftover
    Whole roll contains about 213 pieces.

