# LeetCode

[1. Two Sum](#1-two-sum-1)

[2. Add Two Numbers](#2-add-two-numbers-2)

[3. Longest Substring Without Repeating Characters](#3-longest-substing-without-repeating-characters)

## 1. Two Sum `#1`
### Problem
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

### Example
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

### Solution
```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        list = []
        for i in range(len(nums)):
            for j in range(i + 1, len(nums)):
                if nums[i] + nums[j] == target:
                    list.append(i)
                    list.append(j)
                    return list
```

### Note
Use hash table, maps, etc. for better running time.

May 5th, 2018

## 2. Add Two Numbers `#2`
### Problem
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Example
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

### Solution
```python
# Definition for singly-linked list.
# class ListNode(object):
#    def __init__(self, x):
#        self.val = x
#        self.next = None

class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        firstdigit = l1.val + l2.val
        if firstdigit < 10:
            head = ListNode(firstdigit)
            movepointer = head
            flag = 0
            while l1.next != None:
                if l2.next != None:
                    l1 = l1.next
                    l2 = l2.next
                    digit = l1.val + l2.val
                    if digit < 10:
                        if digit + flag < 10:
                            movepointer.next = ListNode(digit + flag)
                            movepointer = movepointer.next
                            flag = 0
                        else:
                            movepointer.next = ListNode(digit + flag - 10)
                            movepointer = movepointer.next
                            flag = 1
                            movepointer.next = ListNode(1)
                    else:
                        movepointer.next = ListNode(digit - 10 + flag)
                        movepointer = movepointer.next
                        flag = 1
                        movepointer.next = ListNode(1)
                else:
                    l1 = l1.next
                    if l1.val + flag < 10:
                        movepointer.next = ListNode(l1.val + flag)
                        movepointer = movepointer.next
                        flag = 0
                    else:
                        movepointer.next = ListNode(l1.val + flag - 10)
                        movepointer = movepointer.next
                        flag = 1
                        movepointer.next = ListNode(1)
            while l2.next != None:
                l2 = l2.next
                if l2.val + flag < 10:
                    movepointer.next = ListNode(l2.val + flag)
                    movepointer = movepointer.next
                    flag = 0
                else:
                    movepointer.next = ListNode(l2.val + flag - 10)
                    movepointer = movepointer.next
                    flag = 1
                    movepointer.next = ListNode(1)
        else:
            head = ListNode(firstdigit - 10)
            movepointer = head
            movepointer.next = ListNode(1)
            flag = 1
            while l1.next != None:
                if l2.next != None:
                    l1 = l1.next
                    l2 = l2.next
                    digit = l1.val + l2.val
                    if digit < 10:
                        if digit + flag < 10:
                            movepointer.next = ListNode(digit + flag)
                            movepointer = movepointer.next
                            flag = 0
                        else:
                            movepointer.next = ListNode(digit + flag - 10)
                            movepointer = movepointer.next
                            flag = 1
                            movepointer.next = ListNode(1)
                    else:
                        movepointer.next = ListNode(digit - 10 + flag)
                        movepointer = movepointer.next
                        flag = 1
                        movepointer.next = ListNode(1)
                else:
                    l1 = l1.next
                    if l1.val + flag < 10:
                        movepointer.next = ListNode(l1.val + flag)
                        movepointer = movepointer.next
                        flag = 0
                    else:
                        movepointer.next = ListNode(l1.val + flag - 10)
                        movepointer = movepointer.next
                        flag = 1
                        movepointer.next = ListNode(1)
            while l2.next != None:
                l2 = l2.next
                if l2.val + flag < 10:
                    movepointer.next = ListNode(l2.val + flag)
                    movepointer = movepointer.next
                    flag = 0
                else:
                    movepointer.next = ListNode(l2.val + flag - 10)
                    movepointer = movepointer.next
                    flag = 1
                    movepointer.next = ListNode(1)
        return head
```

### Note
Better Solution:

[Pseudocode](https://leetcode.com/problems/add-two-numbers/solution/):
- Initialize current node to dummy head of the returning list.
- Initialize carry to 0.
- Initialize p and q to head of l1 and l2 respectively.
- Loop through lists l1 and l2 until you reach both ends.
  - Set x to node p's value. If p has reached the end of l1, set to 0.
  - Set y to node q's value. If q has reached the end of l2, set to 0.
  - Set sum = x + y + carry.
  - Update carry = sum / 10.
  - Create a new node with the digit value of (sum mod 10) and set it to current node's next, then advance current node to next.
  - Advance both p and q.
- Check if carry = 1, if so append a new node with digit 1 to the returning list.
- Return dummy head's next node.

May 6th, 2018

## 3. Longest Substring Without Repeating Characters `#3`
### Problem
Given a string, find the length of the longest substring without repeating characters.

### Example
```
Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Solution
1. Exceeds Runtime Limit
```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        substr = {}
        max_length = 0
        input_length = len(s)
        for i in range(0, input_length):
            length = 0
            if input_length - i >= max_length:
                for j in range(i, input_length):
                    if not substr.has_key(s[j]):
                        substr[s[j]] = 1
                        length += 1
                    else:
                        substr.clear()
                        break
                max_length = max(length, max_length)
            else:
                break
        return max_length
```

2. Optimized Solution
```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        substr = {}
        str_length = len(s)
        start = 0
        length = 0
        max_length = 0
        for i in range(str_length):
            if not substr.has_key(s[i]):
                substr[s[i]] = i
                length += 1
                max_length = max(max_length, length)
            else:
                for j in range(start, substr[s[i]] + 1):
                    del substr[s[j]]
                    start += 1
                    length -= 1
                substr[s[i]] = i
                length += 1
        return max_length
```

### Note
1. input `pwwkew`<br>
p<br>
pw<br>
pw~~w~~<br>
w<br>
w~~w~~<br>
w<br>
wk<br>
wke<br>
wke~~w~~<br>
output `length = 3`

2. input `gaaqfeqlqky`<br>
g<br>
ga<br>
~~ga~~a<br>
aq<br>
aqf<br>
aqfe<br>
~~aq~~feq<br>
feql<br>
~~feq~~lq<br>
lqk<br>
lqky<br>
output `length = 4`<br>
[Sliding Window](https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/)

May 6th, 2018
