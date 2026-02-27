---
title: Array & String
theme:
  name: catppuccin-mocha
  override:
    code:
      alignment: left
      margin:
        percent: 0
---

# 1768. Merge Strings Alternately

Merge two strings by alternating characters.

```
Input:  word1 = "abc", word2 = "pqr"
Output: "apbqcr"
```

<!-- end_slide -->

## Solution

```python
def mergeAlternately(word1: str, word2: str) -> str:
    result = []
    i, j = 0, 0
    while i < len(word1) and j < len(word2):
        result.append(word1[i])
        result.append(word2[j])
        i += 1
        j += 1
    result.append(word1[i:])
    result.append(word2[j:])
    return "".join(result)
```

<!-- end_slide -->

# 1071. Greatest Common Divisor of Strings

Find the largest string `x` such that `x` divides both `str1` and `str2`.

```
Input:  str1 = "ABCABC", str2 = "ABC"
Output: "ABC"
```

```
Input:  str1 = "ABABAB", str2 = "ABAB"
Output: "AB"
```

<!-- end_slide -->

## Solution

```python
from math import gcd

def gcdOfStrings(str1: str, str2: str) -> str:
    if str1 + str2 != str2 + str1:
        return ""
    return str1[:gcd(len(str1), len(str2))]
```

<!-- end_slide -->

# 1431. Kids With the Greatest Number of Candies

Given `candies[i]` for each kid and `extraCandies`, return a boolean list where `result[i]` is `True` if giving all extra candies to kid `i` would give them the greatest (or tied) number of candies.

```
Input:  candies = [2,3,5,1,3], extraCandies = 3
Output: [true,true,true,false,true]
```

<!-- end_slide -->

## Solution

```python
def kidsWithCandies(candies: list[int], extraCandies: int) -> list[bool]:
    max_candies = max(candies)
    return [c + extraCandies >= max_candies for c in candies]

    # Without list comprehension:
    # result = []
    # for c in candies:
    #     result.append(c + extraCandies >= max_candies)
    # return result
```

<!-- end_slide -->

# 605. Can Place Flowers

Given a flowerbed (0 = empty, 1 = planted) and `n`, return `True` if `n` new flowers can be planted without violating the no-adjacent-flowers rule.

```
Input:  flowerbed = [1,0,0,0,1], n = 1
Output: true
```

```
Input:  flowerbed = [1,0,0,0,1], n = 2
Output: false
```

<!-- end_slide -->

## Solution

Can place a flower at position `i` only if all three are `0`:

```
Case 1: Start of array
  [0] [0] ...
   ^   ^
   i  i+1
   (skip left check)

Case 2: Middle of array
  ... [0] [0] [0] ...
       ^   ^   ^
      i-1  i  i+1
      (check both sides)

Case 3: End of array
  ... [0] [0]
       ^   ^
      i-1  i
      (skip right check)
```

<!-- end_slide -->

## Solution

```python
def canPlaceFlowers(flowerbed: list[int], n: int) -> bool:
    for i in range(len(flowerbed)):
        left = i == 0 or flowerbed[i - 1] == 0
        right = i == len(flowerbed) - 1 or flowerbed[i + 1] == 0
        if flowerbed[i] == 0 and left and right:
            flowerbed[i] = 1
            n -= 1
    return n <= 0
```

<!-- end_slide -->

# 345. Reverse Vowels of a String

Reverse only the vowels in a string.

```
Input:  s = "IceCreAm"
Output: "AcesrеIm"
```

```
Input:  s = "leetcode"
Output: "leotcede"
```

<!-- end_slide -->

## Solution

```python
def reverseVowels(s: str) -> str:
    vowels = set("aeiouAEIOU")
    chars = list(s)
    l, r = 0, len(chars) - 1
    while l < r:
        while l < r and chars[l] not in vowels:
            l += 1
        while l < r and chars[r] not in vowels:
            r -= 1
        chars[l], chars[r] = chars[r], chars[l]
        l += 1
        r -= 1
    return "".join(chars)
```

<!-- end_slide -->

# 238. Product of Array Except Self
`#prefix-sum, #medium`

Return an array where `answer[i]` is the product of all elements except `nums[i]`. **Cannot use division**, must run in O(n).

```
Input:  nums = [1,2,3,4]
Output: [24,12,8,6]
```

<!-- end_slide -->

## Approach 1: Division (breaks with zeros)

```python
def productExceptSelf(nums: list[int]) -> list[int]:
    total = 1
    for n in nums:
        total *= n
    return [total // n for n in nums]
    # Fails when any element is 0 → ZeroDivisionError
```

<!-- end_slide -->

## Approach 2: Two arrays (prefix + suffix)

```
nums   = [1,    2,    3,    4]
left   = [1,    1,    2,    6]   ← product of everything to the left
right  = [24,   12,   4,    1]   ← product of everything to the right
answer = [24,   12,   8,    6]   ← left[i] * right[i]
```

```python
def productExceptSelf(nums: list[int]) -> list[int]:
    n = len(nums)
    left, right, answer = [1] * n, [1] * n, [1] * n

    for i in range(1, n):
        left[i] = left[i - 1] * nums[i - 1]

    for i in range(n - 2, -1, -1):
        right[i] = right[i + 1] * nums[i + 1]

    for i in range(n):
        answer[i] = left[i] * right[i]

    return answer
```

<!-- end_slide -->

## Approach 3: Single array (O(1) extra space)

Use `answer` for prefix, then multiply suffix in-place.

```python
def productExceptSelf(nums: list[int]) -> list[int]:
    n = len(nums)
    answer = [1] * n

    prefix = 1
    for i in range(n):
        answer[i] = prefix
        prefix *= nums[i]

    suffix = 1
    for i in range(n - 1, -1, -1):
        answer[i] *= suffix
        suffix *= nums[i]

    return answer
```

<!-- end_slide -->

# 334. Increasing Triplet Subsequence
`#medium`

Return `True` if there exists `i < j < k` such that `nums[i] < nums[j] < nums[k]`.

```
Input:  nums = [1,2,3,4,5]
Output: true
```

```
Input:  nums = [5,4,3,2,1]
Output: false
```

<!-- end_slide -->

## Solution

Track the smallest and second smallest values seen so far. If we find a third value larger than both, return `True`.

```
nums = [2, 1, 5, 0, 4, 6]

Step 1:  first=2    second=inf
Step 2:  first=1    second=inf   (1 < 2, update first)
Step 3:  first=1    second=5     (5 > 1, update second)
Step 4:  first=0    second=5     (0 < 1, update first)
Step 5:  first=0    second=4     (4 > 0 and 4 < 5, update second)
Step 6:  6 > second → found triplet! return True
```

<!-- end_slide -->

## Solution

```python
def increasingTriplet(nums: list[int]) -> bool:
    first = second = float('inf')
    for n in nums:
        if n <= first:
            first = n
        elif n <= second:
            second = n
        else:
            return True
    return False
```

<!-- end_slide -->

# 443. String Compression

Compress a char array in-place. Consecutive duplicates become the character followed by the count (if > 1). Return the new length.

```
Input:  chars = ["a","a","b","b","c","c","c"]
Output: 6, chars = ["a","2","b","2","c","3"]
```

```
Input:  chars = ["a"]
Output: 1, chars = ["a"]
```

<!-- end_slide -->

## Solution

Use a read pointer to count consecutive chars and a write pointer to overwrite in-place.

```
chars = [a, a, b, b, c, c, c]

read group "a" × 2 → write "a","2"
read group "b" × 2 → write "b","2"
read group "c" × 3 → write "c","3"

result = [a, 2, b, 2, c, 3]  length = 6
```

<!-- end_slide -->

## Solution

```python
def compress(chars: list[str]) -> int:
    write = 0
    read = 0
    while read < len(chars):
        char = chars[read]
        count = 0
        while read < len(chars) and chars[read] == char:
            read += 1
            count += 1
        chars[write] = char
        write += 1
        if count > 1:
            for digit in str(count):
                chars[write] = digit
                write += 1
    return write
```