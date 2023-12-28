---
id: j5nt68t495df32uuynadodo
title: Leet Code
desc: ''
updated: 1703770686757
created: 1703306331660
---

## 21. Merge two LinkedList

2023/12/23 解題思路： 先將兩個LinkedList都轉成`Array`，然後將他們的值加總並排序(`sorted()`)，最後在用這個處理後的`Array`去將每個element map 成 `ListNode`，跑迴圈將每個`ListNode`串接起來

```swift
class Solution {
    func mergeTwoLists(_ list1: ListNode?, _ list2: ListNode?) -> ListNode? {
        func getLinkedArr(_ list: ListNode?) -> [Int] {
            guard let list else { return [] }
            var returnArr: [Int] = []

            if let nextNode = list.next {
                returnArr += [list.val]
                return returnArr + getLinkedArr(nextNode)
            } else {
                return returnArr + [list.val]
            }
        }

        func generateListNode(from array: [Int]) -> ListNode? {
            if array.isEmpty {
                return nil
            }

            var listNodeArr = array.map { ListNode($0) }

            for (index, node) in listNodeArr.enumerated() {
                if index == (listNodeArr.count - 1) {
                    break
                }
                node.next = listNodeArr[index + 1]
            }

            return listNodeArr[0]
        }

        var list1Arr = getLinkedArr(list1)
        var list2Arr = getLinkedArr(list2)
        var sumListArr = (list1Arr + list2Arr).sorted()

        return generateListNode(from: sumListArr)
    }
}
```

反思：
應該應用遞迴的特性，讓listNode去比較並尋找應該接在下一個的listNode，不斷地重複直到沒有下一個listNode.

## 26. Remove Duplicates from Sorted Array

2023/12/24 解題思路：

先宣告一個空的陣列，然後將參數的nums陣列跑迴圈，如果nums陣列中的element沒有包含在substituteNums陣列中，就加進去，否則跳過。

最後再將 substituteNums賦值給nums，並回傳substituteNums的陣列長度。

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        var substituteNums: [Int] = []

        for (index, num) in nums.enumerated() {
            if substituteNums.contains(num) {

            } else {
                substituteNums.append(num)
            }
        }
        nums = substituteNums
        return substituteNums.count
    }
}
```

反思：

```swift
class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        var idx = 0
        for num in nums where num != nums[idx] {
            // 第一個num一定不會被跑到
            idx += 1
            nums[idx] = num
        }

        return idx + 1
    }
}
```