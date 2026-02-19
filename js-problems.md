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