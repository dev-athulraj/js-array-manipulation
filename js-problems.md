# JavaScript Array Problems (Without Built-in Methods)

All problems use manual logic only — no push(), pop(), map(), filter(), etc.

---
**Array:** `[3, 7, 2, 9, 1, 4, 6]`

## Problem 1: Count Even Numbers

**Task:** Count how many even numbers are in the array.

```js
function countEvenNumbers(arr) {
    let evenCount = 0;
    let length = arr.length;

    for (let i = 0; i < length; i++) {
        if (arr[i] % 2 === 0) {
            evenCount++;
        }
    }

    return evenCount;
}

// count === 3
```

## Problem 2: Second largest element.

**Task:** Find the second largest element in the array.

```js
function findSecondLargest(arr) {
    if (arr.length < 2) {
        return null; // Not enough elements
    }

    let first = -Infinity;
    let second = -Infinity;

    for (let i = 0; i < arr.length; i++) {
        let num = arr[i];

        if (num > first) {
            second = first;
            first = num;
        } else if (num < first && num > second) {
            second = num;
        }
    }

    return second === -Infinity ? null : second;
}
```

## Probelem 3: Reverse the first half.

**Task:** Reverse the first half of the array.

```js
function reverseFirstHalf(arr) {
    let left = 0;
    let right = Math.floor(arr.length / 2) - 1;

    while (left < right) {
        let temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;

        left++;
        right--;
    }

    return arr;
}
```

---

## Problem 4: Stable Even-Odd Partition (Order Preserved)

**Array:** `[3, 8, 2, 7, 4, 6, 1]`

**Task:** Separate even and odd numbers such that:
- All even numbers come first
- All odd numbers come after

**Why not swap?**
Swapping disturbs the relative order of one side. Since stability is required, swapping is dangerous — we need a different approach.

**Strategy:**
1. Create a new result array
2. First pass — copy all evens in order
3. Second pass — copy all odds in order

This guarantees full stability with no overwriting, no shifting, no swaps.

```js
function evenFirstStable(arr) {
    let n = arr.length
    let result = new Array(n)
    let index = 0

    // First pass: copy evens
    for (let i = 0; i < n; i++) {
        if (arr[i] % 2 === 0) {
            result[index] = arr[i]
            index++
        }
    }

    // Second pass: copy odds
    for (let i = 0; i < n; i++) {
        if (arr[i] % 2 !== 0) {
            result[index] = arr[i]
            index++
        }
    }

    return result
}
```

**Why this works:**
- First loop copies evens in their original sequence
- Second loop copies odds in their original sequence
- Pure controlled writing — index only moves forward

---

---

## Problem 5: Reverse the Second Half (First Half Unchanged)

**Array:** `[1, 2, 3, 4, 5, 6]`

**Task:** Reverse only the second half of the array. Leave the first half exactly as it is.

**Strategy:**
- Find the midpoint using `Math.floor(n / 2)` — this is where the second half begins
- Place `left` pointer at mid, `right` pointer at last index
- Swap inward using Two Pointer until they meet

```js
function reverseSecondHalf(arr) {
    let n = arr.length
    let left = Math.floor(n / 2)
    let right = n - 1

    while (left < right) {
        let temp = arr[left]
        arr[left] = arr[right]
        arr[right] = temp
        left++
        right--
    }

    return arr
}
```

**Trace through `[1, 2, 3, 4, 5, 6]`:**

| Step | left | right | Array state |
|------|------|-------|-------------|
| Start | 3 | 5 | `[1, 2, 3, 4, 5, 6]` |
| Swap index 3 & 5 | 4 | 4 | `[1, 2, 3, 6, 5, 4]` |
| left === right, stop | — | — | `[1, 2, 3, 6, 5, 4]` |

**Result:** `[1, 2, 3, 6, 5, 4]`

**Why `Math.floor(n / 2)`?**

| Array length | Mid index | First half | Second half |
|--------------|-----------|------------|-------------|
| Even (6) | 3 | index 0–2 | index 3–5 |
| Odd (5) | 2 | index 0–1 | index 2–4 |

For odd-length arrays, the middle element belongs to the second half and gets included in the reversal.

**Pattern:** Bounded Two Pointer — same as Problem 3, but anchored to the second half instead of the first.

**Connects to:** Subarray operations, rotating arrays, palindrome checks on subarrays.

---

## Problem 6: Reverse the Middle (First and Last Unchanged)

**Task:** Reverse the array except the first and last elements. Leave index `0` and index `n-1` untouched.

**Strategy:**
- Start `left` pointer at index `1` (one after first)
- Start `right` pointer at index `n - 2` (one before last)
- Swap inward using Two Pointer until they meet

```js
function reverseMiddle(arr) {
    if (arr.length < 3) return arr

    let start = 1
    let end = arr.length - 2

    while (start < end) {
        let temp = arr[start]
        arr[start] = arr[end]
        arr[end] = temp
        start++
        end--
    }

    return arr
}
```

**Trace through `[1, 2, 3, 4, 5, 6]`:**

| Step | start | end | Array state |
|------|-------|-----|-------------|
| Start | 1 | 4 | `[1, 2, 3, 4, 5, 6]` |
| Swap index 1 & 4 | 2 | 3 | `[1, 5, 3, 4, 2, 6]` |
| Swap index 2 & 3 | 3 | 2 | `[1, 5, 4, 3, 2, 6]` |
| start >= end, stop | — | — | `[1, 5, 4, 3, 2, 6]` |

**Result:** `[1, 5, 4, 3, 2, 6]`

**Why `arr.length < 3` guard?**

| Array | Reason |
|-------|--------|
| `[]` | Nothing to reverse |
| `[1]` | Only one element, no middle exists |
| `[1, 2]` | Only first and last, no middle exists |

Anything less than 3 elements has no middle — return as is.

**Comparison of bounded Two Pointer problems so far:**

| Problem | left starts at | right starts at |
|---------|---------------|-----------------|
| Full reverse | `0` | `n - 1` |
| Reverse first half | `0` | `mid - 1` |
| Reverse second half | `mid` | `n - 1` |
| Reverse middle | `1` | `n - 2` |

**Pattern:** Bounded Two Pointer with anchor exclusion — you control exactly which segment gets reversed by choosing where `left` and `right` start.

---

## Problem 7: Rotate Array Right by 1

**Array:** `[3, 8, 2, 7, 4, 6, 1]`

**Task:** Rotate the array to the right by one position. The last element wraps around to the front.

**Strategy:**
- Save the last element before it gets overwritten
- Shift every element one position to the right (loop backwards)
- Place the saved element at index `0`

**Why loop backwards?**
If we go forward, we overwrite `arr[1]` with `arr[0]` before `arr[1]` is saved — corrupting the data. Going backwards ensures each value is safely moved before its slot is overwritten.

```js
let arr = [3, 8, 2, 7, 4, 6, 1]
let n = arr.length
let last = arr[n - 1]

for (let i = n - 1; i > 0; i--) {
    arr[i] = arr[i - 1]
}

arr[0] = last
```

**Trace through `[3, 8, 2, 7, 4, 6, 1]`:**

| Step | Action | Array state |
|------|--------|-------------|
| Save last | `last = 1` | `[3, 8, 2, 7, 4, 6, 1]` |
| i = 6 | `arr[6] = arr[5]` | `[3, 8, 2, 7, 4, 6, 6]` |
| i = 5 | `arr[5] = arr[4]` | `[3, 8, 2, 7, 4, 4, 6]` |
| i = 4 | `arr[4] = arr[3]` | `[3, 8, 2, 7, 7, 4, 6]` |
| i = 3 | `arr[3] = arr[2]` | `[3, 8, 2, 2, 7, 4, 6]` |
| i = 2 | `arr[2] = arr[1]` | `[3, 8, 8, 2, 7, 4, 6]` |
| i = 1 | `arr[1] = arr[0]` | `[3, 3, 8, 2, 7, 4, 6]` |
| Place last | `arr[0] = 1` | `[1, 3, 8, 2, 7, 4, 6]` |

**Result:** `[1, 3, 8, 2, 7, 4, 6]`

**Direction rule summary so far:**

| Operation | Direction | Reason |
|-----------|-----------|--------|
| Insert at beginning | Backwards | Shift right without overwriting |
| Remove at index | Forwards | Shift left without overwriting |
| Rotate right by 1 | Backwards | Shift right without overwriting |

**Pattern:** Single-save shift — same core idea as Insert at Beginning from the concepts guide, but wrapping instead of inserting.

**Connects to:** Rotate by k steps, circular buffers, sliding window problems.