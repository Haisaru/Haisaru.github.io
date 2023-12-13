+++
title = 'Day 3'
date = 2023-12-03T23:05:27-05:00
draft = false
+++

Today's puzzle was fun. I thought part two was particularly interesting.
The important insight is to realize that any number beside a gear is automatically
a part number because it is a star. Hence, part two boils down to the count of 
times a star is adjacent to a number.

# Part 1
```
def countParts(bp: str):
    # first find locations of all symbols
    # second loop finds numbers adjacent to symbols

    total = 0
    # key is line i and value is line position j
    symbolmap = {}
    for i in range(len(bp)):
        symbolmap[i] = []
    for i, line in enumerate(bp):
        for j, char in enumerate(line):
            if not (char.isdigit() or char == "." or char == "\n"):
                symbolmap[i].append(j)

    # key is tuple (i,j) of first most significant digit and value is length
    digitmap = {}
    i = 0
    j = 0
    while i < len(bp):
        while j < len(bp[i]):
            if bp[i][j].isdigit():
                start = j
                while bp[i][j].isdigit():
                    j += 1
                digitmap[(i, start)] = j - start
            else:
                j += 1
        i += 1
        j = 0
    for number, length in digitmap.items():
        line = number[0]
        pos = number[1]
        value = int(bp[line][pos : pos + length])
        llength = len(bp[line])
        flag = False

        if pos - 1 > -1 and pos - 1 in symbolmap[line]:
            flag = True
        if not flag and pos + length < llength and pos + length in symbolmap[line]:
            flag = True
        if not flag and line > 0:
            for xoffset in range(-1, length + 1):
                newPos = xoffset + pos
                if -1 < newPos < llength and newPos in symbolmap[line - 1]:
                    flag = True
                    break
        if line + 1 < len(bp) and not flag:
            for xoffset in range(-1, length + 1):
                newPos = xoffset + pos
                if -1 < newPos < llength and newPos in symbolmap[line + 1]:
                    flag = True
                    break
        if flag:
            total += value
    return total


if __name__ == "__main__":
    filename = "input.txt"

    with open(filename, encoding="utf-8") as f:
        schematic = f.readlines()
        print(countParts(schematic))
```

# Part 2
```
def countParts(bp: str):
    # first find locations of all symbols
    # second loop finds numbers adjacent to symbols

    total = 0
    # key is line i and value is line position j
    starmap = {}
    starlist = {}
    for i in range(len(bp)):
        starmap[i] = []
    for i, line in enumerate(bp):
        for j, char in enumerate(line):
            if char == "*":
                starmap[i].append(j)
                starlist[(i, j)] = []

    # key is tuple (i,j) of first most significant digit and value is length
    digitmap = {}
    i = 0
    j = 0
    while i < len(bp):
        while j < len(bp[i]):
            if bp[i][j].isdigit():
                start = j
                while bp[i][j].isdigit():
                    j += 1
                digitmap[(i, start)] = j - start
            else:
                j += 1
        i += 1
        j = 0
    for number, length in digitmap.items():
        line = number[0]
        pos = number[1]
        value = int(bp[line][pos : pos + length])
        llength = len(bp[line])

        if pos - 1 > -1 and pos - 1 in starmap[line]:
            starlist[(line, pos - 1)].append(value)
        if pos + length < llength and pos + length in starmap[line]:
            starlist[(line, pos + length)].append(value)
        if line > 0:
            for xoffset in range(-1, length + 1):
                newPos = xoffset + pos
                if -1 < newPos < llength and newPos in starmap[line - 1]:
                    starlist[(line - 1, newPos)].append(value)
        if line + 1 < len(bp):
            for xoffset in range(-1, length + 1):
                newPos = xoffset + pos
                if -1 < newPos < llength and newPos in starmap[line + 1]:
                    starlist[(line + 1, newPos)].append(value)
    for parts in starlist.values():
        if len(parts) == 2:
            total = total + (parts[0] * parts[1])
    return total


if __name__ == "__main__":
    filename = "input.txt"

    with open(filename, encoding="utf-8") as f:
        schematic = f.readlines()
        print(countParts(schematic))
```

