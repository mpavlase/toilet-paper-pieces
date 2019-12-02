```python
from math import pi
```


```python
# all numbers are in mm!

d_inner = 40
d_outer = 105
piece_length = 138
thikness = 0.265

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
print(f'Sum pieces: {sum_pieces:1.3f}')
```

    Expected end about 2.390 pieces/round
    Round 121: 2.395 pieces/round with -1.395 leftover
    Sum pieces: 201.245

