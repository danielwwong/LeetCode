# LeetCode

[1. Two Sum `#1`](#1-two-sum-1)

[2. Add Two Numbers `#2`](#2-add-two-numbers-2)

[3. Longest Substring Without Repeating Characters `#3`](#3-longest-substring-without-repeating-characters-3)

[4. Median of Two Sorted Arrays `#4`](#4-median-of-two-sorted-arrays-4)

[5. Longest Palindromic Substring `#5`](#5-longest-palindromic-substring-5)

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

## 4. Median of Two Sorted Arrays `#4`
### Problem
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

### Example
- Example 1:
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```

- Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

### Solution
```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        len_a = len(nums1)
        len_b = len(nums2)
        if ((len_a + len_b) % 2) == 1: #one odd, one even
            k = (len_a + len_b + 1) / 2
            return self.find_k(k, nums1, nums2, len_a, len_b)
        else:
            k_1 = (len_a + len_b) / 2
            k_2 = (len_a + len_b) / 2 + 1
            array_1 = nums1[:]
            array_2 = nums2[:]
            i = self.find_k(k_1, nums1, nums2, len_a, len_b)
            j = self.find_k(k_2, array_1, array_2, len_a, len_b)
            return (float(i) + j) / 2

    def find_k(self, k, nums1, nums2, len_a, len_b):
        if len_a == 0:
            return nums2[k - 1]
        elif len_b == 0:
            return nums1[k - 1]
        elif (len_a % 2) == 1 and (len_b % 2) == 1: #two odd
            x = (len_a - 1) / 2
            y = (len_b - 1) / 2
            position = x + y + 1
            if nums1[x] > nums2[y]:
                array_a = nums2
                array_b = nums1
                temp = x
                x = y
                y = temp
            else:
                array_a = nums1
                array_b = nums2
            if k <= position:
                del array_b[y : len_b]
                return self.find_k(k, array_a, array_b, len(array_a), len(array_b))
            else:
                del array_a[0 : (x + 1)]
                return self.find_k(k - x - 1, array_a, array_b, len(array_a), len(array_b))
        elif (len_a % 2) == 0 and (len_b % 2) == 0: #two even
            x = len_a / 2 - 1
            y = len_b / 2
            position = x + y + 1
            if nums1[x] > nums2[y]:
                array_a = nums2
                array_b = nums1
                temp = x
                x = y
                y = temp
            else:
                array_a = nums1
                array_b = nums2
            if k <= position:
                del array_b[y : len_b]
                return self.find_k(k, array_a, array_b, len(array_a), len(array_b))
            else:
                del array_a[0 : (x + 1)]
                return self.find_k(k - x - 1, array_a, array_b, len(array_a), len(array_b))
        else:
            if k == 1:
                return min(nums1[0], nums2[0])
            else:
                if nums1[0] > nums2[0]:
                    array_a = nums2
                    array_b = nums1
                    return self.find_k(k, array_a, array_b, len(array_a), len(array_b))
                else:
                    array_a = nums1
                    array_b = nums2
                    del array_a[0]
                    return self.find_k(k - 1, array_a, array_b, len(array_a), len(array_b))
```

### Note
Extend the problem to find the k-th number of the two sorted arrays:

**Explanation**

[Reference](http://eriol.iteye.com/blog/1172098)

For instance,

```
A = [1, 2, 5, 6, 9, 12]
B = [3, 7, 8, 10, 11, 15]
A[0] = 1, B[0] = 3
```

Split each arrays into two equal-long subarrays.

```
A = [1, 2, 5 | 6, 9, 12]
B = [3, 7, 8 | 10, 11, 15]
```

Split the each array equally.

In A, the first part has **x** numbers, the second part has **m-x** numbers, A[x] is the first number in the second part. x = 3, A[x] = 6, m = 6.

Similarly, in B, the first part has **y** numbers, the second part has **n-y** numbers, B[y] is the first number in the second part. y = 3, B[y] = 10, n = 6.

If A[x] <= B[x], continue. If A[x] > B[x], we switch the arrays.

- Scenario 1<br>
In A, **x** numbers <= **A[x]**. In B, **y** numbers <= **B[y]**. And **A[x]** <= **B[y]**.<br>
Hence, there're **x + y + 1** numbers <= B[y].<br>
If **k** <= **x + y + 1**, the k-th number must be in front of **B[y]** (not including B[y]).<br>
Hence, we can discard the second part of B, which is B[y] and the numbers after B[y].

- Scenario 2<br>
In A, **m - x - 1** numbers >= **A[x]**. In B, **n - y - 1** numbers >= **B[y]**. And **A[x]** <= **B[y]**.<br>
Hence, there're (m - x - 1) + (n - y - 1) + 1 = **(m + n) - (x + y + 1)** numbers >= A[x].<br>
If **k** > **x + y + 1**, the k-th number must be after **A[x]** (not including A[x]).<br>
Hence, we can discard the first part of A, which is A[x] and the numbers before A[x].

**Boundary Problem**

[Reference](https://blog.csdn.net/lqglqglqg/article/details/48845225)

There're different situations we should consider, for instance, if the array's length is odd or even, or the array is shorter than <img src="img/1.png" height="38px">. We need to handle the boundary problem.

- Scenario 1<br>
**both arrays' lengths are odd**<br>
Set the median of a as a[x], the median of b as b[y].<br>
![img](img/6.jpg)<br>
The length of array a is **2x + 1**, the length of array b is **2y + 1**. Total length is **2(x + y + 1)**.
  - If k <= x + y + 1, we can discard the yellow part. Blue part below is x + y + 1. Then, we find the **k-th** number in the new arrays.<br>
  ![img](img/7.jpg)
  - If k > x + y + 1, we can discard the yellow part. Blue part below is x + y + 1. Then, we find the **(k - x - 1)-th** number in the new arrays.<br>
  ![img](img/8.jpg)
  - If a[x] > b[y], we simply switch the arrays.

- Scenario 2<br>
**both arrays' lengths are even**<br>
Set the **upper** median of a as a[x], the **lower** median of b as b[y].<br>
![img](img/9.jpg)<br>
The length of array a is **2x + 2**, the length of array b is **2y**. Total length is **2(x + y + 1)**.
  - If k <= x + y + 1, we can discard the yellow part. Blue part below is x + y + 1. Then, we find the **k-th** number in the new arrays.<br>
  ![img](img/10.jpg)
  - If k > x + y + 1, we can discard the yellow part. Blue part below is x + y + 1. Then, we find the **(k - x - 1)-th** number in the new arrays.<br>
  ![img](img/11.jpg)
  - If a[x] > b[y], we simply switch the arrays.

- Scenario 3<br>
**one of the arrays' length is odd, the other one is even**
  - If k = 1, we return min(a[0], b[0]).
  - If k > 1, and if a[0] < b[0], a[0] would be the smallest number of the two arrays. We discard the a[0], then find the **(k - 1)-th** number in the new arrays. Array a's length will be subtracted by 1, the problem will be transformed to Scenario 1 or Scenario 2.

**Others**

Finding the median of the two sorted arrays can be treated as
1. finding the <img src="img/2.png" height="38px">-th number as **i**, and <img src="img/3.png" height="38px">-th number as **j**, of the two sorted arrays, then the median is <img src="img/4.png" height="38px">, if **m + n** is even;
2. finding the <img src="img/5.png" height="38px">-th number of the two sorted arrays, if **m + n** is odd.

**Important Stuff**
1. Need to check on finding k-th number, or (k - x - 1)-th number, or (k - 1)-th number.
2. Need to check on k and the subscript. The array starts from 0, but k starts form 1.
3. Python's variable is only a name tag. The array after inputting to the functions will be altered globally. Copy the arrays if needed. Use `copy_list = original_list[:]` to copy. Knows what it means and how to use slice operator. `:` in Python is a slice operator.
4. In Python, if you want to delete some list elements. For instance, for `a = [1, 2, 3]`, `del a[0 : 2]` will result in `a = [3]`. For `a = [1]`, `del a[0 : 1]` will result in `a = []`.
5. Return the value from recursion should be handled. It will only return to one upper layer, will not return to the top layer. We should return the value recursively, too.

May 7th, 2018

## 5. Longest Palindromic Substring `#5`
### Problem
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

### Example
- Example 1:
```
Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

- Example 2:
```
Input: "cbbd"
Output: "bb"
```

### Solution
1. Exceeds Runtime Limit
```python
class Solution(object):
    def longestPalindrome(self, s):
        """
        :type s: str
        :rtype: str
        """
        new_s = '\0'
        for i in range(len(s) - 1):
            new_s = new_s + s[i] + '\1'
        new_s = new_s + s[len(s) - 1] + '\0'
        length = []
        for i in range(len(new_s)):
            if new_s[i] == '\0':
                continue
            else:
                j = 1
                radius = 0
                while new_s[i - j] != '\0' and new_s[i + j] != '\0' and new_s[i - j] == new_s[i + j]:
                    if new_s[i + j] != '\1':
                        radius += 2
                    j += 1
                if new_s[i] != '\1':
                    radius += 1
                length.append(radius)
        result = []
        if (max(length) % 2) == 1:
            for i in range(max(length)):
                result.append(s[length.index(max(length)) / 2 - max(length) / 2 + i])
        else:
            for i in range(max(length)):
                result.append(s[(length.index(max(length)) + 1) / 2 - max(length) / 2 + i])
        result_str = ''.join(result)
        return result_str
```

### Note
1. - Handle the Original String<br>
Example:<br>
Original String: babad<br>
After Handled: $b#a#b#a#d$<br>
In Solution 1, I used '\0' to be '$' and '\1' to be '#' in order to avoid the situation that if the original string have the character '$' or '#'.<br>
After handling the string, we don't need to check about whether the palindromic string is odd or even.
  - When hitting '\0' ('$'), means it reaches to the edge, stop.
  - Record the length of the palindromic by adding 2 when having the same character in both of the side.<br>
  If having '\1' ('#') in both of the side, do not add numbers.<br>
  In the end, check the middle point. If the middle point is not '\1' ('#'), add 1.<br>
  It will have the length of the palindromic string in the original string.<br>
  ```
  a b a b d
  $ a # b # a # b # d $
  - 1 0 3 0 3 0 1 0 1 -
  ```

May 9th, 2018
