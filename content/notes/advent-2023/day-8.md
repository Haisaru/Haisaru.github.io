+++
title = 'Day 8'
date = 2023-12-13T14:02:16-05:00
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
if __name__ == "__main__":
    from math import lcm

    filename = "input.txt"
    with open(filename, encoding="utf-8") as f:
        network = {}
        inst = f.readline().strip()
        a_nodes = []
        f.readline()
        for line in f:
            curr, next_up = line.split("=")
            curr = curr.strip()
            next_up = next_up.strip()
            network[curr] = (next_up[1:4], next_up[6:9])
            if curr[2] == "A":
                a_nodes.append(curr)
        counter = 0
        finals = [False for _ in range(6)]
        inst_index = 0
        inst_len = len(inst)
        while not all(finals):
            counter += 1
            for i, node in enumerate(a_nodes):
                if inst[inst_index % inst_len] == "L":
                    a_nodes[i] = network[node][0]
                else:
                    a_nodes[i] = network[node][1]
                if a_nodes[i][2] == "Z" and not finals[i]:
                    print(finals)
                    finals[i] = counter
            inst_index += 1
        print(finals)
        print(lcm(*finals))
```
