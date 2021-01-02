---
title: LV 1. Athelete who could not Finish
tags: [Algorithm]
style: border
color: info
description: This question is from Programmers and code is by Joonho Lee. If you have any other ideas or questions about my code, please leave a comment below.
---

Source: [Programmers](https://programmers.co.kr/)

---

## Problem Description

Lots of maraton runners participate the marathon. All but one athlete completed the marathon.

There is given array **participant** which contains the names of the atheletes who participated in the maraton and **completion** which contains the names of the atheletes who finished the marathon. Please write the function that return the name of an athlete who could not finish the marathon.

## Restrictions

- The number of atheletes participating in the marathon is greater than or equal to 1, and less than or equal to 10,0000.
- The length of completion is always 1 less than the length of the participant.
- The length of names in participant are greater than or equal to 1 and less than or equal to 20. (all names are writeen by lowercase alphabetic characters).
- Some of the participants may have the same name.

## I/O Example

| participant                                       | completion                               | return   |
| ------------------------------------------------- | ---------------------------------------- | -------- |
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"    |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"  |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav" |

## I/O Example Explanation

###### Example #1

"leo" was on the list of participant, but could not finish because "leo" was not on the list of completion.

###### Example #2

"vinko" was on the list of participant, but could not finish because "vinko" was not on the list of completion.

###### Example #3

two "mislav" was on the list of participant, but one could not finish because there is only one "mislav" on the list of completion.

## Code

```python
def solution(participant, completion):

    participant.sort()
    completion.sort()
    length = len(participant)
    for i in range(length):
        if i == length-1 or participant[i] != completion[i]:
            return participant[i]
```

## Test

```python
print(solution(["leo", "kiki", "eden"], ["eden", "kiki"]))
print(solution(["marina", "josipa", "nikola", "vinko", "filipa"], ["josipa", "filipa", "marina", "nikola"]))
print(solution(["mislav", "stanko", "mislav", "ana"], ["stanko", "ana", "mislav"]))
```

    "leo"
    "vinko"
    "mislav"
