+++
title = 'Day 1'
date = 2023-12-01T18:55:24-05:00
draft = false
type = "page"
+++

Part 1 was much more straightforward for me than part 2. I initially tried an iterative approach but got tangled in the implementation.
I tried a regex approach but got stuck on the "eighttwothree" string for a while. It wasn't until I found the ?= look ahead
operator in regex that I finished the solution.

# Part 1
```
filename = "input1.txt"
with open(filename, encoding="utf-8") as f:
    total = 0
    for line in f:
        left = 0
        right = len(line) - 1
        while (not line[left].isdigit()):
            left += 1
        while (not line[right].isdigit()):
            right -= 1
        total += int(line[left]+line[right])
    print(total)
```
# Part 2
```
import re


def convert_str_to_int(number):
    try:
        return int(number)
    except ValueError:
        return digits.index(number)


if __name__ == "__main__":
    filename = "input2.txt"
    digits = ['zero', 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine']

    # match substring if and only if the following characters is a digit, or one, two,..., nine
    match_num = re.compile(r'(?=(\d|one|two|three|four|five|six|seven|eight|nine))', re.IGNORECASE)

    with open(filename, encoding="utf-8") as f:
        total = 0
        for line in f:
            matches = match_num.findall(line)
            left = matches[0]
            right = matches[-1]
            left = convert_str_to_int(left)
            right = convert_str_to_int(right)
            concat = str(left) + str(right)
            total += int(concat)
        print(total)
```
