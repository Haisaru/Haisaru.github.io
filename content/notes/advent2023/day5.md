+++
title = 'Day 5'
date = 2023-12-06T10:05:44-05:00
draft = false
+++

Today we continue the pattern of easy then hard question.
It took me a while to track down a bug in part 1 of my code
where I created 7 aliases for a dictionary instead of creating 
7 separate dictionaries. 

For part two, I considered implementing an algorithm faster than brute force.
However, it took a while to get the range `mapRangesToValues` working as intended.
The tricky part was considering all the ways the algorithm should start, terminate,
and increment.

# Part 1

```
def addLineToMap(line: list[int], map: dict) -> None:
    offset = line[0] - line[1]
    domainSize = line[2]
    # values in [line[1], line[1] + domainSize) get moved by offset
    map[(line[1], line[1] + domainSize)] = offset


def mapToNewValues(values: list[int], map: dict) -> list[int]:
    for idx, value in enumerate(values):
        for domain, offSet in map.items():
            end = domain[1]
            start = domain[0]
            if start <= value < end:
                values[idx] = value + offSet
                break
    return values


if __name__ == "__main__":
    filename = "input.txt"

    with open(filename, encoding="utf-8") as f:
        # note [{}] * 7 creates 7 reference to the same object
        allMaps = [{} for _ in range(7)]
        mapNum = 0
        flag = False
        domain = []
        for lineNum, line in enumerate(f):
            if lineNum == 0:
                domain = [int(seed) for seed in line[7:].split()]
            elif flag:
                if not line[0].isdigit():
                    mapNum += 1
                    flag = False
                else:
                    map = allMaps[mapNum]
                    line = [int(num) for num in line.split()]
                    addLineToMap(line, map)
                continue
            else:
                if not line[0].isdigit():
                    continue
                else:
                    flag = True
                    map = allMaps[mapNum]
                    line = [int(num) for num in line.split()]
                    addLineToMap(line, map)
        for map in allMaps:
            domain = mapToNewValues(domain, map)
        print(min(domain))
```

# Part 2

```
def addLineToMap(line: list[int], map: list) -> None:
    offset = line[0] - line[1]
    domainSize = line[2]
    # values in [line[1], line[1] + domainSize) get moved by offset
    map.append((line[1], line[1] + domainSize, offset))


def mapRangesToValues(ranges: list[(int, int)], map: dict) -> list[int]:
    newRanges = []
    i = 0
    j = 0
    start1, end1 = ranges[0]
    start2, end2, offset = map[0]
    if start2 > start1:
        newRanges.append((start1, start2))

    while i < len(ranges) and j < len(map):
        # Check for none empty intersection
        start = max(start1, start2)
        end = min(end1, end2)
        if start < end:
            newRanges.append((start + offset, end + offset))
        # Move to the next interval in the list that has a smaller end value
        if end1 < end2:
            i += 1
            if i >= len(ranges):
                break
            start1, end1 = ranges[i]
        elif end1 >= end2:
            j += 1
            if j >= len(map):
                if end2 != end1 and end2 >= start1:
                    newRanges.append((end2, end1))
                    i += 1
                elif end2 == end1:
                    i += 1
                break
            start2, end2, offset = map[j]
            if start2 >= end1:
                newRanges.append((start1, end1))
    while i < len(ranges):
        newRanges.append(ranges[i])
        i += 1

    newRanges.sort(key=lambda interval: interval[0])

    return newRanges


def seedsToRange(values: list[int]) -> list[(int, int)]:
    i = 0
    returned = []
    while (i * 2) + 1 < len(values):
        start = values[i * 2]
        ammount = values[(i * 2) + 1]
        returned.append((start, start + ammount))
        i += 1
    return returned


if __name__ == "__main__":
    filename = "input.txt"

    with open(filename, encoding="utf-8") as f:
        # note [{}] * 7 creates 7 reference to the same object
        allMaps = [[] for _ in range(7)]
        mapNum = 0
        flag = False
        domain = []
        for lineNum, line in enumerate(f):
            if lineNum == 0:
                domain = seedsToRange([int(seed) for seed in line[7:].split()])
                domain = sorted(domain, key=lambda interval: interval[0])
            elif flag:
                if not line[0].isdigit():
                    mapNum += 1
                    flag = False
                else:
                    map = allMaps[mapNum]
                    line = [int(num) for num in line.split()]
                    addLineToMap(line, map)
                continue
            else:
                if not line[0].isdigit():
                    continue
                else:
                    flag = True
                    map = allMaps[mapNum]
                    line = [int(num) for num in line.split()]
                    addLineToMap(line, map)
        for map in allMaps:
            map.sort(key=lambda entry: entry[0])
        for map in allMaps:
            domain = mapRangesToValues(domain, map)
        print(domain[0][0])
```
