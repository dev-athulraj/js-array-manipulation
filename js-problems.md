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