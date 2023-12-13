+++
title = 'Day 4'
date = 2023-12-06T10:05:38-05:00
draft = false
+++

Not much comment on today's puzzle. It was a just an easy task
of reading the lines and getting the right data format. From
there a quick application of set intersection completes the question.

# Part 1
```
def winnings(line: str) -> int:
    i = 0
    while line[i] != ":":
        i += 1
    line = line[i + 2 :]
    winningNums, givenNums = line.split("|")
    winningNums = set(winningNums.split())
    givenNums = set(givenNums.split())

    winners = len(winningNums & givenNums)
    if winners == 0:
        return 0
    else:
        return pow(2, winners - 1)


if __name__ == "__main__":
    filename = "input.txt"

    with open(filename, encoding="utf-8") as f:
        total = 0
        for line in f:
            total += winnings(line)
        print(total)
```

# Part 2

```
def winnings(line: str) -> int:
    i = 0
    while line[i] != ":":
        i += 1
    line = line[i + 2 :]
    winningNums, givenNums = line.split("|")
    winningNums = set(winningNums.split())
    givenNums = set(givenNums.split())

    return len(winningNums & givenNums)


if __name__ == "__main__":
    filename = "input.txt"

    with open(filename, encoding="utf-8") as f:
        copies = {}
        total = 0
        for id, line in enumerate(f, 1):
            winners = winnings(line)
            for i in range(1, winners + 1):
                copies[id + i] = copies.get(id, 1) + copies.get(id + i, 1)
        gamecount = id
        total = 0
        for i in range(gamecount):
            total += copies.get(i + 1, 1)
        print(total)
```
