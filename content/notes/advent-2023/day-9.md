+++
title = 'Day 9'
date = 2023-12-13T14:02:21-05:00
draft = false
+++

# Part 1

```
if __name__ == "__main__":
    filename = "input.txt"
    with open(filename, encoding="utf-8") as f:
        network = {}
        inst = f.readline().strip()
        f.readline()
        for line in f:
            curr, next_up = line.split("=")
            next_up = next_up.strip()
            network[curr.strip()] = (next_up[1:4], next_up[6:9])
        curr = "AAA"
        counter = 0
        inst_index = 0
        inst_len = len(inst)
        while curr != "ZZZ":
            if inst[inst_index % inst_len] == "L":
                curr = network[curr][0]
            else:
                curr = network[curr][1]
            counter += 1
            inst_index += 1
        print(counter)

```

# Part 2

```
def extrapolate(history: str) -> int:
    history = history.split()
    history = [int(num) for num in history]

    calcs = []
    calc = history
    while True:
        calcs.append(calc)
        calc = [calc[i + 1] - calc[i] for i in range(len(calc) - 1)]
        if all([num == 0 for num in calc]):
            break
    extrapolation = 0
    for calc in reversed(calcs):
        extrapolation = calc[0] - extrapolation
    return extrapolation


if __name__ == "__main__":
    filename = "input.txt"
    with open(filename, encoding="utf-8") as f:
        total = 0
        for line in f:
            total += extrapolate(line)
        print(total)

```
