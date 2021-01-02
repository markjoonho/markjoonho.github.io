---
title: LV 2. Phone Number List
tags: [Fast Campus ML]
style: border
color: info
description: This question is from Programmers and code is by Joonho Lee. If you have any other ideas or questions about my code, please leave a comment below.
---

Source: [Programmers](https://programmers.co.kr/)

## Problem Description

I want to check if there are any phone numbers listed in the phone book is a prefix of another number.
If the phone number is as follows, the phone number of the rescue team is the prefix of Yeong Seok's phone number.

- Rescue Team: 119
- Park Joonyoung: 97674223
- Ji Yeong Seok: 1195524421

When the array phone_book containing the phone numbers which are in the phone book is given as a parameter of the solution function, write the solution function to return false if a number is a prefix of another number, and true otherwise.

---

## Restriction

- The length of phone_book is greater than or equal to 1, and less than or equal to 1,000,000.

- Each phone numbers' length is greater than or equal to 1, and less than or equal to 20.

---

## I/O Example

| phone_book                        | return |
| :-------------------------------- | :----- |
| ["119", "97674223", "1195524421"] | false  |
| ["123","456","789"]               | true   |
| ["12","123","1235","567","88"]    | false  |

---

## I/O Example Explanation

###### Example 1

Same as the example from the problem description.

###### Example 2

Since one number is never a prefix of another, the answer is true.

###### Example 3

The first phone number, “12” is the prefix for the second phone number “123”. So the answer is false.

---

## Code

```python
def solution(phone_book):
    ls = sorted(phone_book)
    num = len(phone_book)
    if num == 0:
        return True
    for i in range(num):
        k = i+1
        while k < num:
            if ls[i] == ls[k][:len(ls[i])]:
                return False
            else:
                return True
            k += 1
        return True
```

## Test

```python
# Test Cases
print(solution(["119", "97674223", "1195524421"]))
print(solution(["123","456","789"]))
print(solution(["12","123","1235","567","88"]))
```

    False
    True
    False
