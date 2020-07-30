---
layout: post
title: Only update nxl header when fileinfo or token change
---

[toc]

## Prepare

### Issue

1. After modify filename(fileExtension...), FileInfo in nxl header not update .
2. when move nxl from project to workspace, nxl have to encrypt again. 

### Requirement

1. update nxl header when file property change.
2. only update nxl header when token change.
3. update nxl header when membership change.
4. update nxl header when policy or tag change.

### Solution

1. when file property change:
   1. rewrite **fileinfo** in *section data*.
   2. update **fileinfo record** in *section header*.
   3. update **header hash** in *dynamic header*.
2. when token change:
   1. decrypt CEK with old token, then encrypt and  get hash with new token.
   2. modify  **TP CEK, TP CEK Checksum, RPP CEK, RPP CEK Checksum, Public Key #1, Public Key #2, Token Level** in *key header*. 
   3. modify **DUID** in *file header*.
   4. recalcuate **checksum** in *section header*.
   5. update **header hash** in *dynamic header*.
3. when membership change:
   1. modify **owner's id** in *file header*.
   2. update **header hash** in *dynamic header*.
4. when policy or tag change:
   like change file propery.



## Doing

### Role

| Responsibility | Person  |
| -------------- | ------- |
| Owner          | pchen   |
| Coder          | pchen   |
| Tester         | unknown |

### Plan

| Date      | Target                                                       | Person  |
| --------- | ------------------------------------------------------------ | ------- |
| 8.3 ~8.7  | update nxl header when fileinfo(membership, policy, tag) change. | pchen   |
| 8.7~8.14  | update nxl header with new token.                            | pchen   |
| 8.17~8.28 | test                                                         | unknown |

### Updating

*Pending.*

## Done

*Pending.*



[wiki]: pending