+++
title = 'Day 6'
date = 2023-12-06T10:05:52-05:00
draft = false
+++

Today's question was a nice application of quadratics.
A bit of care is needed when the roots of the quadratic 
are integer. I wonder if there's a better way to deal with strict 
inequalities than how I did so below.

# Part 1

```
import cmath
from math import floor, ceil

filename = "input.txt"

with open(filename, encoding="utf-8") as f:
    times = f.readline()
    i = 0
    while not times[i].isdigit():
        i += 1
    times = [int(time) for time in times[i:].split()]
    distances = f.readline()
    i = 0
    while not distances[i].isdigit():
        i += 1
    distances = [int(distance) for distance in distances[i:].split()]

    i = 0
    total = 1
    while i < len(times):
        time = times[i]
        distance = distances[i]
        rdiscrim = cmath.sqrt(time**2 - 4 * distance)

        root1 = (time - rdiscrim) / 2
        root2 = (time + rdiscrim) / 2
        if root1 == root2:
            pass
        elif root1.imag != 0:
            print(0)
            exit()
        else:
            root1 = root1.real
            root2 = root2.real
            if (root1 == ceil(root1)):
                root1 += 1
            else:
                root1 = ceil(root1)
            if (root2 == floor(root2)):
                root2 -= 1
            else:
                root2 = floor(root2)
            total *= root2 - root1 + 1
        i += 1
    print(total)
```

# Part 2

```
import cmath
from math import floor, ceil

filename = "input.txt"

with open(filename, encoding="utf-8") as f:
    time = f.readline()
    i = 0
    while not time[i].isdigit():
        i += 1
    time = int(time[i:].replace(" ", ""))
    distance = f.readline()
    i = 0
    while not distance[i].isdigit():
        i += 1
    distance = int(distance[i:].replace(" ", ""))

    rdiscrim = cmath.sqrt(time**2 - 4 * distance)

    root1 = (time - rdiscrim) / 2
    root2 = (time + rdiscrim) / 2
    if root1 == root2:
        print(0)
        exit()
    elif root1.imag != 0:
        print(0)
        exit()
    else:
        root1 = root1.real
        root2 = root2.real
        if root1 == ceil(root1):
            root1 += 1
        else:
            root1 = ceil(root1)
        if root2 == floor(root2):
            root2 -= 1
        else:
            root2 = floor(root2)
        print(root2 - root1 + 1)
```
