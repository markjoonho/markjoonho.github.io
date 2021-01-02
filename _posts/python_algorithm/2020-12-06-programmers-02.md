---
title: LV 2. Lifeboat
tags: [Algorithm]
style: border
color: info
description: This question is from Programmers and code is by Joonho Lee. If you have any other ideas or questions about my code, please leave a comment below.
---

Source: [Programmers](https://programmers.co.kr/)

---

## Problem Description

They are trying to rescue people trapped on an island using a lifeboat. Lifeboats are small and can only ride up to two people at a time, and they have weight restrictions. For example, if people's weight is [70kg, 50kg, 80kg, 50kg] and the weight limit of the lifeboat is 100kg, the second person and the fourth person can ride together, but the first person and the third person cannot ride together since the their weights over the limit of the lifeboat. Try to rescue everyone by using as few lifeboats as possible. Given an array of people's weights and the weight limit of lifeboats as parameters, write the solution function to return the minimum number of lifeboats needed to rescue everyone.

---

## Restriction

- There are more than or equal to 1 person and less than or equal to 50,000 people trapped on a desert island.
- Each people's weights are greater than or equal to 40kg and less than or equal to 240kg.
- The weight limit for life boat is greater than or equal to 40kg and less than or equal to 240kg.
- The weight limit of a lifeboat is always higher than the maximum weight of a person.

---

## I/O Example

| people           | limit | return |
| ---------------- | ----- | ------ |
| [70, 50, 80, 50] | 100   | 3      |
| [70, 80, 50]     | 100   | 3      |

---

## Code

```python
def solution(people, limit):
    people.sort()
    start = 0
    end = len(people) - 1
    answer = 0

    while start <= end:
        if people[start] + people[end] <= limit:
            start += 1
            end -= 1
        else:
            end -= 1
        answer += 1
    return answer
```

## Test

```python
# Test Cases
print(solution([70, 50, 80, 50], 100))
print(solution([70, 80, 50], 100))
```

    3
    3
