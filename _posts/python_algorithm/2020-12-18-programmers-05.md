---
title: LV 2. English Endings
tags: [Algorithm]
style: border
color: info
description: This question is from Programmers and code is by Joonho Lee. If you have any other ideas or questions about my code, please leave a comment below.
---

Source: [Programmers](https://programmers.co.kr/)

---

## Problem description

There are n people with numbers from 1 to n doing English Endings game. English endings are conducted according to the following rules.

1. Beginning with number 1, say the words one by one in number order.
2. After the last person has spoken the word, it starts again at number 1.
3. You must say words that start with the last letter of the word spoken by the previous person.
4. Words that have appeared previously cannot be used.
5. Single letter words are not accepted.

The following shows a situation where 3 people talk to each other.

```
tank → kick → know → wheel → land → dream → mother → robot → tank
```

The ending above proceeds as follows.

- Person 1 says tank on his first turn.
- Person 2 says kick on his first turn.
- Person 3 says know on his first turn.
- Person 1 says wheel on his second turn.
- (Continue on)

If you continue to do the ending, the word tank that person 3 spoke on his third turn is a word that appeared before, so it will be eliminated.

When the number of people n and the words words spoken by people in order are given as parameters, complete the solution function so that the number of the first person to drop out and the number of times that person is eliminated are returned.

---

## Restrictions

- The number of people participating in the ending n is a natural number from 2 to 10.
- words is an array containing the words used for ending in order, and the length is n or more and 100 or less.
- The length of the word is 2 or more and 50 or less.
- All words consist of only lowercase letters of the alphabet.
- You don't have to worry about the meaning (meaning) of the words used for ending.
- Please return the correct answer in the form of [number, order].
- If there are no dropouts for the given words, return [0, 0].

---

## I/O example

| n   | words                                                                                                                                | result |
| --- | ------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| 3   | [tank, kick, know, wheel, land, dream, mother, robot, tank]                                                                          | [3,3]  |
| 5   | [hello, observe, effect, take, either, recognize, encourage, ensure, establish, hang, gather, refer, reference, estimate, executive] | [0,0]  |
| 2   | [hello, one, even, never, now, world, draw]                                                                                          | [1,3]  |

---

## I/O example explanation

#### I/O Example #1

3 people are participating in the closing.

- Person 1: tank, wheel, mother
- Person 2: kick, land, robot
- Person 3: know, dream, tank

The words are spoken in the same order as and the word "spoken by person #3 on their tankthird turn is the tanksame as that from person #1 on their first turn , so when person #3 speaks in their third turn," The first dropouts will come out.

#### I/O Example #2

5 people are participating in the closing.

- Person 1: hello, recognize, gather
- Person 2: observe, encourage, refer
- Person 3: effect, ensure, reference
- Person 4: take, establish, estimate
- Person 5: either, hang, executive

The words are spoken in the same order, and in this case, no dropouts occur only with the given word. So, just return [0, 0].

#### I/O Example #3

Two people are participating in the closing.

- Person 1: hello, even,, nowdraw
- Person 2: one, never, world

The words are spoken in the same order as, and the nowfirst dropout comes out at this time because person number 1 said the word starting with n instead of the word starting with'r' in his third turn .

---

## Code

```python
def solution(n, words):
    answer = [0,0]
    total_words = len(words)

    for i in range(total_words):
        if i == total_words - 1:
            break
        if words[i][-1] == words[i+1][0]:
            if words[i+1] in words[:i+1]:
                answer[1] = (i + 1) // n + 1
                answer[0] = (i + 1) % n + 1
                return answer
            else:
                pass
        else:
            answer[1] = (i + 1) // n + 1
            answer[0] = (i + 1) % n + 1
            return answer
    return answer
```
