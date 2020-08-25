---
layout: post
title: Coding Test Study
mathjax: true
tags:
  - python
---

<br/>

## Intro 문제
### 1번
```python
def add(param1, param2):
  return param1 + param2
```
<br/>

### 2번
```python
def centuryFromYear(year):
    cent = year // 100
    if year % 100 == 0:
        return cent
    else:
        return cent + 1
```
<br/>

### 3번
```python
def checkPalindrome(inputString):
    if len(inputString) == 1:
        return True
    elif len(inputString) != 1:
        mid = len(inputString) // 2
        for i in range(mid):
            if inputString[i] != inputString[-1-i]:
                return False
        return True
```
<br/>

### 4번
```python
def adjacentElementsProduct(inputArray):
    max_num = inputArray[0] * inputArray[1]
    return_idx1, return_idx2 = 0, 1
    for i in range(1, len(inputArray)-1):
        if inputArray[i] * inputArray[i+1] > max_num:
            max_num = inputArray[i] * inputArray[i+1]
            return_idx1, return_idx2 = i, i+1
        elif inputArray[i-1] * inputArray[i] > max_num:
            max_num = inputArray[i-1] * inputArray[i]
            return_idx1, return_idx2 = i-1, i
    return inputArray[return_idx1] * inputArray[return_idx2]
```
<br/>

### 5번 (20.06.05)
```python
def shapeArea(n):
    sums = 0
    for i in range(1, n+1):
        sums+=i
    return sums*4 - (n*4-1)
```
<br/>

### 6번 (20.06.16)
```python
def makeArrayConsecutive2(statues):
    return_num = 0
    statues.sort()
    for i in range(len(statues)-1):
        if statues[i+1] - statues[i] != 1:
            return_num += statues[i+1] - statues[i] - 1
    return return_num
```
<br/>

### 7번 (20.06.26)
```python
def almostIncreasingSequence(sequence):
    cnt = 0
    i = 1
    first = sequence[0]
    while i <= len(sequence):
        if sequence[i-1] >= sequence[i]:
            cnt += 1
            if sequence[i-1] != first:
                if sequence[i-2] >= sequence[i]:
                    sequence.pop(i)
                else:
                    sequence.pop(i-1)
            else:
                sequence.pop(i-1)
            if sorted(set(sequence)) == sequence:
                return True
            else:
                return False
        i += 1
    if cnt < 1:
        return True
    else:
        return False
```
<br/>

### 8번(20.06.28)
```python
def matrixElementsSum(matrix):
    tran_mat = [ list(x) for x in zip(*matrix) ]
    sum_num = 0
    for lists in tran_mat:
        nums = 0
        for i in lists:
            if i != 0:
                nums += i
            else:
                break
        sum_num += nums
    return sum_num
```
<br/>

### 9번(20.07.19)
```python
def allLongestStrings(inputArray):
    max_len = 0
    for i in inputArray:
        if len(i) > max_len:
            max_len = len(i)
    return_array = []
    for j in inputArray:
        if len(j) == max_len:
            return_array.append(j)
    return return_array
```
<br/>

### 10번(20.08.03)
```python
def commonCharacterCount(s1, s2):
    s1_dic = {}; s2_dic = {}
    for i in set(s1):
        s1_dic[i] = 0
    for i in set(s2):
        s2_dic[i] = 0
    for j in s1:
        s1_dic[j] = s1_dic[j] + 1
    for j in s2:
        s2_dic[j] = s2_dic[j] + 1
    
    return_sum = 0
    for i in s1_dic.keys():
        for j in s2_dic.keys():
            if i == j:
                if s1_dic[i] < s2_dic[j]:
                    return_sum += s1_dic[i]
                elif s1_dic[i] >= s2_dic[j]:
                    return_sum += s2_dic[j]
    return return_sum
```
<br/>

### 11번(20.08.10)
```python
def isLucky(n):
    left = str(n)[:len(str(n))//2]
    right = str(n)[len(str(n))//2:]
    left_sum = sum([int(i) for i in left])
    right_sum = sum([int(i) for i in right])
    if left_sum == right_sum:
        return True
    else:
        return False

```
<br/>

### 12번(20.08.25)
```python
def sortByHeight(a):
    sort_list = []
    for i in a:
        if i != -1:
            sort_list.append(i)
    sort_list = sorted(sort_list)
    return_list = []
    for j in a:
        if j == -1:
            return_list.append(j)
        else:
            return_list.append(sort_list[0])
            sort_list.pop(0)
    return return_list
```
