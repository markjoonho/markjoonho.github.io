---
title: LV 2. The Largest Number
tags: [Fast Campus ML]
style: border
color: info
description: This question is from Programmers and code is by Joonho Lee. If you have any other ideas or questions about my code, please leave a comment below.
---

Source: [Programmers](https://programmers.co.kr/)

---

## Problem Description

Given a zero or positive integer, concatenate the integers together to find out the largest number you can make.

For example, if the given integer is [6, 10, 2], you can make [6102, 6210, 1062, 1026, 2610, 2106], and the largest from this is 6210.

Write the solution function to return the largest number that can be made by rearranging the order as a string when an array **numbers** containing zero or positive integers is given as a parameter.

---

## Restrictions

- The length of **numbers** is greater than or equal to 1, and less than or equal to 10,0000.
- Each elements of **numbers** is greater than or equal to 1, and less than or equal to 1000.
- The correct answer may be too large, so convert it to a string and return.

---

## I/O Example

| numbers           | return    |
| ----------------- | --------- |
| [6, 10, 2]        | "6210"    |
| [3, 30, 34, 5, 9] | "9534330" |

---

## Code

```python
def solution(numbers):
    answer = ''
    lst = []
    for num in numbers:
        lst.append(str(num))

    lst.sort(key = lambda x:x*3, reverse=True)
    answer = str(int(''.join(lst)))
    return answer
```

## Test

```python
print(solution([6, 10, 2]))
print(solution([3, 30, 34, 5, 9]))
```

    "6210"
    "9534330"
