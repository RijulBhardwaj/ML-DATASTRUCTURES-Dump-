Nice, let’s build you a mini **DSA cheat sheet** with **super common interview questions**, each:

* explained like you’re 10 😄
* broken down **step by step**
* with **simple code** (Python, but logic is language-independent)

We’ll do 6 classics:

1. Two Sum (array + hashmap)
2. Maximum Subarray (Kadane’s Algorithm)
3. Reverse a Linked List
4. Valid Parentheses (Stack)
5. Merge Two Sorted Arrays
6. Binary Search

---

## 1️⃣ Two Sum (Array + Hashmap)

### ❓ Problem

Given an array and a target, return **indices of two numbers** that add up to the target.
Example: `nums = [2, 7, 11, 15], target = 9` → answer: indices `[0, 1]` (because 2 + 7 = 9)

### 💡 Idea (in simple words)

We scan the array once and remember numbers we’ve seen in a **map (dictionary)**:

* For each number `x`, we need `target - x`
* If we’ve already seen `target - x`, we found the pair
* Else, store `x` in a dictionary: `value → index`

### 🧠 Step-by-step (dry run)

`nums = [2, 7, 11, 15], target = 9`

| i | nums[i] | need = target - nums[i] | Seen Map (`value: index`) | Found?        |
| - | ------- | ----------------------- | ------------------------- | ------------- |
| 0 | 2       | 7                       | {}                        | 7 not in map  |
|   |         |                         | {2: 0}                    |               |
| 1 | 7       | 2                       | {2: 0}                    | 2 **in map!** |

So answer: `[ map[2], 1 ] = [0, 1]`

### 🧾 Code (Python)

```python
def two_sum(nums, target):
    index_map = {}  # value -> index
    
    for i, num in enumerate(nums):
        need = target - num
        if need in index_map:
            return [index_map[need], i]
        index_map[num] = i
    
    return None  # if no pair
```

---

## 2️⃣ Maximum Subarray (Kadane’s Algorithm)

### ❓ Problem

Given an array (can have negative numbers), find the **maximum possible sum** of a **contiguous** subarray.

Example: `[-2, 1, -3, 4, -1, 2, 1, -5, 4]` → max sum = `6` (subarray `[4, -1, 2, 1]`)

### 💡 Idea

We walk through the array and keep two values:

* `curr_sum` = best sum ending **at this index**
* `max_sum` = best sum seen **so far**

At each step:

* Either **extend** the previous sum: `curr_sum + num`
* Or **start fresh** from current number: `num`
  Pick whichever is bigger.

### 🧠 Step-by-step

Array: `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

| i | num | curr_sum = max(num, curr_sum + num) | max_sum |
| - | --- | ----------------------------------- | ------- |
| 0 | -2  | max(-2, -2) = -2                    | -2      |
| 1 | 1   | max(1, -2+1=-1) = 1                 | 1       |
| 2 | -3  | max(-3, 1-3=-2) = -2                | 1       |
| 3 | 4   | max(4, -2+4=2) = 4                  | 4       |
| 4 | -1  | max(-1, 4-1=3) = 3                  | 4       |
| 5 | 2   | max(2, 3+2=5) = 5                   | 5       |
| 6 | 1   | max(1, 5+1=6) = 6                   | 6       |
| 7 | -5  | max(-5, 6-5=1) = 1                  | 6       |
| 8 | 4   | max(4, 1+4=5) = 5                   | 6       |

Answer = `max_sum = 6`

### 🧾 Code

```python
def max_subarray(nums):
    curr_sum = nums[0]
    max_sum = nums[0]
    
    for num in nums[1:]:
        curr_sum = max(num, curr_sum + num)
        max_sum = max(max_sum, curr_sum)
    
    return max_sum
```

---

## 3️⃣ Reverse a Singly Linked List

### ❓ Problem

Given the head of a linked list, reverse the list.
Example: `1 -> 2 -> 3 -> 4 -> None` → `4 -> 3 -> 2 -> 1 -> None`

### 💡 Idea (pointer flip)

We walk through the list and **flip the arrows**:

We keep:

* `prev` (starts as `None`)
* `curr` (starts as `head`)

At each step:

1. Save next node: `next_node = curr.next`
2. Reverse pointer: `curr.next = prev`
3. Move `prev` to `curr`
4. Move `curr` to `next_node`

When `curr` becomes `None`, `prev` is the new head.

### 🧠 Step-by-step (small example)

List: `1 -> 2 -> 3 -> None`

Start: `prev = None`, `curr = 1`

* Step 1:

  * `next_node = 2`
  * `1.next = None` (reverse)
  * `prev = 1`
  * `curr = 2`
* Step 2:

  * `next_node = 3`
  * `2.next = 1`
  * `prev = 2`
  * `curr = 3`
* Step 3:

  * `next_node = None`
  * `3.next = 2`
  * `prev = 3`
  * `curr = None` → stop

New list: `3 -> 2 -> 1 -> None`

### 🧾 Code (Python, assuming class ListNode)

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverse_list(head):
    prev = None
    curr = head
    
    while curr is not None:
        next_node = curr.next   # 1) save next
        curr.next = prev        # 2) reverse link
        prev = curr             # 3) move prev
        curr = next_node        # 4) move curr
    
    return prev  # new head
```

---

## 4️⃣ Valid Parentheses (Stack)

### ❓ Problem

Given a string with brackets `()`, `{}`, `[]`, check if it’s **valid**:

* Every open has a matching close
* Order is correct

Example:

* `"()[]{}"` → valid
* `"(]"` → invalid
* `"({[]})"` → valid

### 💡 Idea

Use a **stack**:

* When you see an **opening bracket**, push it to stack
* When you see a **closing bracket**:

  * Stack must not be empty
  * Top of stack must be the matching opening
* In the end, stack must be empty

### 🧠 Step-by-step

Input: `"({[]})"`

Stack initially: `[]`

| char | Action                    | Stack       |
| ---- | ------------------------- | ----------- |
| (    | open → push               | [ ( ]       |
| {    | open → push               | [ (, { ]    |
| [    | open → push               | [ (, {, [ ] |
| ]    | close → matches `[` → pop | [ (, { ]    |
| }    | close → matches `{` → pop | [ ( ]       |
| )    | close → matches `(` → pop | []          |

Stack empty → valid ✅

### 🧾 Code

```python
def is_valid(s):
    stack = []
    match = {')': '(', '}': '{', ']': '['}
    
    for ch in s:
        if ch in "({[":
            stack.append(ch)
        else:  # closing bracket
            if not stack:
                return False
            if stack[-1] != match[ch]:
                return False
            stack.pop()
    
    return len(stack) == 0
```

---

## 5️⃣ Merge Two Sorted Arrays

### ❓ Problem

Given two **sorted** arrays, merge them into **one sorted** array.

Example:
`a = [1, 3, 5]`
`b = [2, 4, 6]`
→ `[1, 2, 3, 4, 5, 6]`

### 💡 Idea (two pointers)

Use two indices:

* `i` for `a`, `j` for `b`
* Compare `a[i]` and `b[j]`, pick smaller, move that pointer
* When one finishes, append the rest of the other array

### 🧠 Step-by-step

`a = [1, 3, 5]`, `b = [2, 4, 6]`

| result      | i | j | a[i] | b[j] | action                     |
| ----------- | - | - | ---- | ---- | -------------------------- |
| []          | 0 | 0 | 1    | 2    | take 1 (from a)            |
| [1]         | 1 | 0 | 3    | 2    | take 2 (from b)            |
| [1,2]       | 1 | 1 | 3    | 4    | take 3                     |
| [1,2,3]     | 2 | 1 | 5    | 4    | take 4                     |
| [1,2,3,4]   | 2 | 2 | 5    | 6    | take 5                     |
| [1,2,3,4,5] | 3 | 2 | -    | 6    | a finished → add rest of b |

→ `[1,2,3,4,5,6]`

### 🧾 Code

```python
def merge_sorted(a, b):
    i = j = 0
    result = []
    
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            result.append(a[i])
            i += 1
        else:
            result.append(b[j])
            j += 1
    
    # Add remaining elements
    while i < len(a):
        result.append(a[i])
        i += 1
    
    while j < len(b):
        result.append(b[j])
        j += 1
    
    return result
```

---

## 6️⃣ Binary Search (on Sorted Array)

### ❓ Problem

Given a **sorted** array and a target value, return its index, or `-1` if not found.

Example:
`nums = [1, 3, 5, 7, 9], target = 7` → index `3`

### 💡 Idea

Instead of scanning one by one, **cut the search space in half** each time:

1. Find middle element
2. If middle == target → done
3. If target < middle → search left half
4. If target > middle → search right half

### 🧠 Step-by-step

`nums = [1, 3, 5, 7, 9], target = 7`

* `left = 0, right = 4`
* mid = (0+4)//2 = 2 → nums[2] = 5

  * 7 > 5 → search right: left = 3, right = 4
* mid = (3+4)//2 = 3 → nums[3] = 7

  * found at index 3 ✅

### 🧾 Code

```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif target < nums[mid]:
            right = mid - 1
        else:
            left = mid + 1
    
    return -1
```

---

If you want, next I can:

* Turn this into a **GitHub-ready cheat-sheet README**
* Or give you **practice problems + solutions** topic-wise:

  * Arrays/strings
  * Linked lists
  * Stacks/queues
  * Trees/graphs
  * DP only (with patterns)

Tell me which topic you want to go deep into first, and I’ll make a focused pack for that.
