---
id: y4x1bbpym9389xf1pj71s7z
title: Performance
desc: ''
updated: 1698795421437
created: 1698475925104
---

## Dimensions of Performance

1. Allocation
   1. stack & heap
2. Reference Counting
3. Method Dispatch

### Allocation

記憶體儲存的位置，

如果沒有identity, indirect storage的需求，使用struct會比class有更好的效能。

#### Dynamic / Static Dispatch

- Dynamic:
  1. Table Dispatch(witness table)
  2. Message Dispatch(KVO, Core Data)
- Static:
  1.