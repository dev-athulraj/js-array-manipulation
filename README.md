# JavaScript Array Manipulation Without Built-in Methods

---

## What an Array Really Is in JavaScript

**Conceptually:**

```js
let arr = [10, 20, 30]
```

**Internally, you can think of it as:**

| Index | Value |
|-------|-------|
| 0     | 10    |
| 1     | 20    |
| 2     | 30    |

`arr.length === 3`

You access elements via `arr[i]`. That's it.

Everything else (`push`, `pop`, `map`...) is just abstraction over this.

---

## Core Mental Model

When you manipulate arrays manually, you must answer:

1. Where am I **reading** from?
2. Where am I **writing** to?
3. Am I **overwriting** something important?
4. Do I need a **new array** or can I modify in place?

This thinking connects directly to:

- Memory models
- Two pointer technique
- Sliding window
- In-place algorithms
- Time and space complexity

We will reuse the same example array throughout:

```js
let arr = [3, 7, 2, 9, 1]
```

---

## 1. Traversing an Array (Foundation of Everything)

```js
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i])
}
```

Why `i < arr.length`? Because the last valid index is always `length - 1`.

This single loop is the base of:

- Searching
- Counting
- Filtering
- Mapping
- Reversing
- Sorting

**Everything is traversal.**

---

## 2. Manual Push (Append to End)

**Built-in:**

```js
arr.push(5)
```

**Manual:**

```js
arr[arr.length] = 5
```

**Why this works:** If `arr.length` is `5`, the last occupied index is `4`, so index `5` is empty — we place the value there. JavaScript automatically increases `length` as a result.

---

## 3. Manual Pop (Remove Last)

**Built-in:**

```js
arr.pop()
```

**Manual:**

```js
let lastIndex = arr.length - 1
let removed = arr[lastIndex]
arr.length = lastIndex
```

**Why this works:** Reducing `arr.length` truncates the array.

> 💡 Key insight: `Array.length` is writable.

---

## 4. Insert at Beginning (Shift Right)

Goal:
```
[3, 7, 2, 9, 1]  →  [100, 3, 7, 2, 9, 1]
```

**Steps:**
1. Shift everything right by one index
2. Insert the new value at index `0`

```js
for (let i = arr.length; i > 0; i--) {
    arr[i] = arr[i - 1]
}

arr[0] = 100
```

**Why loop backwards?** Going forward would overwrite values before they're moved, corrupting your data.

> 💡 Key insight: **Direction of traversal matters.**

This concept appears in: reversal, removing elements, rotations, and merging arrays.

---

## 5. Remove at Index (Shift Left)

Remove the element at index `2` from `[3, 7, 2, 9, 1]`.

**Steps:**
1. Shift everything after the target index left by one
2. Reduce `length` by one

```js
let removeIndex = 2

for (let i = removeIndex; i < arr.length - 1; i++) {
    arr[i] = arr[i + 1]
}

arr.length = arr.length - 1
```

**Result:** `[3, 7, 9, 1]`

> 💡 Again: direction matters — we go **forward** here because we're shifting left.

---

## 6. Manual Reverse (Two Pointer Technique)
```
[3, 7, 2, 9, 1]  →  [1, 9, 2, 7, 3]
```

```js
let left = 0
let right = arr.length - 1

while (left < right) {
    let temp = arr[left]
    arr[left] = arr[right]
    arr[right] = temp

    left++
    right--
}
```

This introduces the **Two Pointer Pattern**, used in:

- Palindrome problems
- Sorted pair sum
- Removing duplicates
- Partitioning arrays

---

## 7. Manual Search (Linear Search)

Find if `9` exists in the array:

```js
let target = 9
let found = false

for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
        found = true
        break
    }
}
```

**Time Complexity:** O(n)

---

## 8. Manual Max Element

```js
let max = arr[0]

for (let i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i]
    }
}
```

This is called the **Running Comparison Pattern**, used in:

- Finding min / max
- Second largest element
- Frequency counting

---

## 9. Manual Filter (Create New Array)

Goal: Keep only numbers greater than `5`.
```js
let newArr = []
let newIndex = 0

for (let i = 0; i < arr.length; i++) {
    if (arr[i] > 5) {
        newArr[newIndex] = arr[i]
        newIndex++
    }
}
```

> 💡 We manually control where we write using `newIndex`. This is the core of filter logic.

This pattern appears in: removing duplicates, partitioning even/odd, removing specific elements.

---

## 10. Manual Map (Transform Values)

Goal: Multiply each element by `2`.
```js
let result = []

for (let i = 0; i < arr.length; i++) {
    result[i] = arr[i] * 2
}
```

**Map vs Filter — the key difference:**

| Method | Output Size |
|--------|-------------|
| Map    | Always same size as input |
| Filter | Equal to or smaller than input |