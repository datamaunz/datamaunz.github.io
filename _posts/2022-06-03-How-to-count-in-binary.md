--- 
layout: post 
title: How to count in binary
subtitle: Notes from Neil Anderson's CCNA Course
categories: CCNA-4-OSI-Layer-3-Network-Layer
tags: [ccna, networking, binary, counting]
---

## Fundamentals

- computers work in binary
- electrical impulses are either off or on (two choices: 0 or 1)
- per *column* in a number, we have two possible choices: 0 or 1
- every time a column is added to the left, its value is multiplied by 2

| 256 | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (236) | 1 (108) | 1 (44) | 1 (12) | 0 (12) | 1 (4) | 1 (0) | 0 (0) | 0 (0) |

- 128 + 64 + 32 + 8 + 4 = 236 
- 236 in binary is thus: 11101100

| 256 | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0 (179) | 1 (51) | 0 (51) | 1 (19) | 1 (3) | 0 (3) | 0 (3) | 1 (1) | 1 (0) |

- 128 + 32 + 16 + 2 + 1 = 179
- 179 in binary is thus: 10110011