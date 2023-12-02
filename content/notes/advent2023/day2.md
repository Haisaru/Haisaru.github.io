+++
title = 'Day 2'
date = 2023-12-02T01:46:47-05:00
draft = false
+++

Unlike day 2, I found the second part very quickly after having the first part done.

# Part 1
```
redCount = 12
greenCount = 13
blueCount = 14


def checkValue(line: str) -> int:
    line = line[5:]
    if line[1] == ":":
        gameIndex = int(line[0])
        line = line[3:]
    else:
        gameIndex = int(line[0] + line[1])
        line = line[4:]
    idx = 0
    while idx < len(line):
        if (not line[idx].isdigit()):
            idx += 1
            continue
        currCount = ""
        while line[idx].isdigit():
            currCount = currCount + line[idx]
            idx += 1
        idx += 1
        if line[idx] == "r":
            if int(currCount) > redCount:
                return 0
        elif line[idx] == "g":
            if int(currCount) > greenCount:
                return 0
        elif line[idx] == "b":
            if int(currCount) > blueCount:
                return 0

    return gameIndex


if __name__ == "__main__":
    filename = "input1.txt"

    with open(filename, encoding="utf-8") as f:
        total = 0
        for line in f:
            total += checkValue(line)
        print(total)
```

# Part 2
```
def checkPower(line: str) -> int:
    line = line[5:]
    if line[1] == ":":
        line = line[3:]
    else:
        line = line[4:]
    idx = 0
    maxRed = 0
    maxGreen = 0
    maxBlue = 0
    while idx < len(line):
        if not line[idx].isdigit():
            idx += 1
            continue
        currCount = ""
        while line[idx].isdigit():
            currCount = currCount + line[idx]
            idx += 1
        idx += 1
        if line[idx] == "r":
            maxRed = max(maxRed, int(currCount))
        elif line[idx] == "g":
            maxGreen = max(maxGreen, int(currCount))
        elif line[idx] == "b":
            maxBlue = max(maxBlue, int(currCount))

    return maxRed * maxGreen * maxBlue


if __name__ == "__main__":
    filename = "input2.txt"

    with open(filename, encoding="utf-8") as f:
        total = 0
        for line in f:
            total += checkPower(line)
        print(total)
```
