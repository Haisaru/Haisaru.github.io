+++
title = 'Day 7'
date = 2023-12-13T14:02:13-05:00
draft = false
+++

Part 1 was a good opportunity for me to practice implementing custom comparison functions
and how to use them as keys for sorted in Python. It was also a good practice of using the 
Counter class. The second part was a straight forward extension of the first but had some edge
cases to be beware in implementation. The key observation that the best use of Jacks is always
adding it to the card type with the highest count.

# Part 1

```
from collections import Counter
from functools import cmp_to_key


def cmp(hand1: list[str], hand2: list[str]) -> int:
    """
    each hand is a tuple [cards, bid]
    return -1 if hand1 < hand2
    return 0 if hand1 == hand2
    return 1 if hand1 > hand2
    """
    cardToRank = {
        "2": 1,
        "3": 2,
        "4": 3,
        "5": 4,
        "6": 5,
        "7": 6,
        "8": 7,
        "9": 8,
        "T": 9,
        "J": 10,
        "Q": 11,
        "K": 12,
        "A": 13,
    }
    cards1 = list(hand1[0])
    cards2 = list(hand2[0])

    cards1 = [cardToRank[card] for card in cards1]
    cards2 = [cardToRank[card] for card in cards2]

    handtype1 = handType(cards1)
    handtype2 = handType(cards2)
    if handtype1 > handtype2:
        return 1
    elif handtype2 > handtype1:
        return -1
    else:
        for card1, card2 in zip(cards1, cards2):
            if card1 == card2:
                continue
            elif card1 > card2:
                return 1
            else:
                return -1
        # should not get here for our purposes
        return 0


def handType(hand: list[str]) -> int:
    """
    hand is a list of chars representing the characters
    returns the type of the hand as an int
    7: five of a kind
    6: four of a kind
    5: full house
    4: three of a kind
    3: two pair
    2: one pair
    1: high card
    """
    sorted_counts = Counter(hand).most_common()
    if sorted_counts[0][1] == 5:
        return 7
    if sorted_counts[0][1] == 4:
        return 6
    if sorted_counts[0][1] == 3 and sorted_counts[1][1] == 2:
        return 5
    if sorted_counts[0][1] == 3:
        return 4
    if sorted_counts[0][1] == 2 and sorted_counts[1][1] == 2:
        return 3
    if sorted_counts[0][1] == 2:
        return 2
    if sorted_counts[0][1] == 1:
        return 1


if __name__ == "__main__":
    filename = "input.txt"
    hands = []
    with open(filename, encoding="utf-8") as f:
        for line in f:
            splitline = line.split()
            hands.append((splitline[0], int(splitline[1])))
        sorted_hands = sorted(hands, key=cmp_to_key(cmp))
        total = 0
        for rank, play in enumerate(sorted_hands, 1):
            total += rank * play[1]
        print(total)

```

# Part 2

```
from collections import Counter
from functools import cmp_to_key


def cmp(hand1: list[str], hand2: list[str]) -> int:
    """
    each hand is a tuple [cards, bid]
    return -1 if hand1 < hand2
    return 0 if hand1 == hand2
    return 1 if hand1 > hand2
    """
    cardToRank = {
        "2": 1,
        "3": 2,
        "4": 3,
        "5": 4,
        "6": 5,
        "7": 6,
        "8": 7,
        "9": 8,
        "T": 9,
        "J": 0,
        "Q": 11,
        "K": 12,
        "A": 13,
    }
    cards1 = list(hand1[0])
    cards2 = list(hand2[0])

    cards1 = [cardToRank[card] for card in cards1]
    cards2 = [cardToRank[card] for card in cards2]

    handtype1 = handType(cards1)
    handtype2 = handType(cards2)
    if handtype1 > handtype2:
        return 1
    elif handtype2 > handtype1:
        return -1
    else:
        for card1, card2 in zip(cards1, cards2):
            if card1 == card2:
                continue
            elif card1 > card2:
                return 1
            else:
                return -1
        # should not get here for our purposes
        return 0


def handType(hand: list[str]) -> int:
    """
    hand is a list of chars representing the cards
    returns the type of the hand as an int
    7: five of a kind
    6: four of a kind
    5: full house
    4: three of a kind
    3: two pair
    2: one pair
    1: high card
    """
    counts = Counter(hand)
    # as Jokers have rank 0 in the card rankings
    joker_count = counts[0]
    if joker_count == 5:
        return 7
    if joker_count != 0:
        two_most_common = counts.most_common(2)
        if two_most_common[0][0] != 0:
            subtracted = Counter({two_most_common[0][0]: -joker_count, 0: joker_count})
            counts.subtract(subtracted)
        else:
            subtracted = Counter({two_most_common[1][0]: -joker_count, 0: joker_count})
            counts.subtract(subtracted)
    sorted_counts = counts.most_common()

    if sorted_counts[0][1] == 5:
        return 7
    if sorted_counts[0][1] == 4:
        return 6
    if sorted_counts[0][1] == 3 and sorted_counts[1][1] == 2:
        return 5
    if sorted_counts[0][1] == 3:
        return 4
    if sorted_counts[0][1] == 2 and sorted_counts[1][1] == 2:
        return 3
    if sorted_counts[0][1] == 2:
        return 2
    if sorted_counts[0][1] == 1:
        return 1


if __name__ == "__main__":
    filename = "input.txt"
    hands = []
    with open(filename, encoding="utf-8") as f:
        for line in f:
            splitline = line.split()
            hands.append((splitline[0], int(splitline[1])))
        sorted_hands = sorted(hands, key=cmp_to_key(cmp))
        total = 0
        for rank, play in enumerate(sorted_hands, 1):
            total += rank * play[1]
        print(total)
```
