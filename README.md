# Custom Implementations of JavaScript Built-in Functions

This document contains hand-crafted custom implementations of major JavaScript built-in functions, grouped by type. Each method includes a **function signature**, **code**, and a **detailed dry run explanation** to help you understand the internal logic step by step.

---

## 📦 Array Methods (with Detailed Dry Run)

### 1. `Array.prototype.myMap`
```js
Array.prototype.myMap = function(callback, thisArg) {
  let result = [];
  for (let i = 0; i < this.length; i++) {
    if (!(i in this)) continue;  // Skip holes in sparse arrays
    result[i] = callback.call(thisArg, this[i], i, this);
  }
  return result;
};
```
**Dry Run**:
```js
const arr = [1, 2, 3];
const output = arr.myMap(x => x * 2);
// Iteration:
// i = 0 → callback(1, 0, arr) => 2 → result = [2]
// i = 1 → callback(2, 1, arr) => 4 → result = [2, 4]
// i = 2 → callback(3, 2, arr) => 6 → result = [2, 4, 6]
```

---

### 2. `Array.prototype.myFilter`
```js
Array.prototype.myFilter = function(callback, thisArg) {
  let result = [];
  for (let i = 0; i < this.length; i++) {
    if (callback.call(thisArg, this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};
```
**Dry Run**:
```js
const arr = [1, 2, 3, 4];
const output = arr.myFilter(x => x % 2 === 0);
// i = 0 → 1 % 2 === 0 → false
// i = 1 → 2 % 2 === 0 → true → result = [2]
// i = 2 → 3 % 2 === 0 → false
// i = 3 → 4 % 2 === 0 → true → result = [2, 4]
```

---

### 3. `Array.prototype.myReduce`
```js
Array.prototype.myReduce = function(callback, initialValue) {
  let acc = initialValue;
  let startIndex = 0;
  if (acc === undefined) {
    acc = this[0];
    startIndex = 1;
  }
  for (let i = startIndex; i < this.length; i++) {
    acc = callback(acc, this[i], i, this);
  }
  return acc;
};
```
**Dry Run**:
```js
const arr = [1, 2, 3];
const sum = arr.myReduce((acc, cur) => acc + cur, 0);
// acc = 0
// i = 0 → acc = 0 + 1 = 1
// i = 1 → acc = 1 + 2 = 3
// i = 2 → acc = 3 + 3 = 6
```

---

### 4. `Array.prototype.myForEach`
```js
Array.prototype.myForEach = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    callback.call(thisArg, this[i], i, this);
  }
};
```
**Dry Run**:
```js
const arr = [10, 20];
arr.myForEach((value, index) => {
  console.log(`Index ${index}:`, value);
});
// Index 0: 10
// Index 1: 20
```

---

### 5. `Array.prototype.mySort`
```js
Array.prototype.mySort = function(compareFn) {
  for (let i = 0; i < this.length; i++) {
    for (let j = i + 1; j < this.length; j++) {
      if ((compareFn && compareFn(this[i], this[j]) > 0) ||
          (!compareFn && String(this[i]) > String(this[j]))) {
        [this[i], this[j]] = [this[j], this[i]];
      }
    }
  }
  return this;
};
```
**Dry Run**:
```js
const arr = [3, 1, 2];
arr.mySort();
// i=0, j=1 → compare(3,1) → swap → [1,3,2]
// j=2 → compare(1,2) → no swap
// i=1, j=2 → compare(3,2) → swap → [1,2,3]
```

---

### 6. `Array.prototype.myPush`
```js
Array.prototype.myPush = function(...items) {
  for (let i = 0; i < items.length; i++) {
    this[this.length] = items[i];
  }
  return this.length;
};
```
**Dry Run**:
```js
const arr = [1, 2];
arr.myPush(3, 4); // arr becomes [1, 2, 3, 4], returns 4
```

---

### 7. `Array.prototype.myPop`
```js
Array.prototype.myPop = function() {
  if (this.length === 0) return undefined;
  const item = this[this.length - 1];
  this.length--;
  return item;
};
```
**Dry Run**:
```js
const arr = [1, 2, 3];
const popped = arr.myPop(); // popped = 3, arr = [1, 2]
```

---

### 8. `Array.prototype.myShift`
```js
Array.prototype.myShift = function() {
  if (this.length === 0) return undefined;
  const item = this[0];
  for (let i = 1; i < this.length; i++) {
    this[i - 1] = this[i];
  }
  this.length--;
  return item;
};
```
**Dry Run**:
```js
const arr = [10, 20, 30];
const shifted = arr.myShift(); // shifted = 10, arr = [20, 30]
```

---

### 9. `Array.prototype.myUnshift`
```js
Array.prototype.myUnshift = function(...args) {
  const newLength = this.length + args.length;
  for (let i = newLength - 1; i >= args.length; i--) {
    this[i] = this[i - args.length];
  }
  for (let i = 0; i < args.length; i++) {
    this[i] = args[i];
  }
  return newLength;
};
```
**Dry Run**:
```js
const arr = [3, 4];
const len = arr.myUnshift(1, 2); // arr = [1, 2, 3, 4], len = 4
```

---

### 10. `Array.prototype.myIncludes`
```js
Array.prototype.myIncludes = function(value) {
  for (let i = 0; i < this.length; i++) {
    if (this[i] === value || (Number.isNaN(this[i]) && Number.isNaN(value))) return true;
  }
  return false;
};
```
**Dry Run**:
```js
const arr = [1, 2, NaN];
arr.myIncludes(NaN); // true
arr.myIncludes(2);   // true
arr.myIncludes(5);   // false
```

---

### 11. `Array.prototype.myEvery`
```js
Array.prototype.myEvery = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    if (!callback.call(thisArg, this[i], i, this)) {
      return false;
    }
  }
  return true;
};
```
**Dry Run**:
```js
const arr = [2, 4, 6];
arr.myEvery(x => x % 2 === 0); // true
```

---

### 12. `Array.prototype.mySome`
```js
Array.prototype.mySome = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    if (callback.call(thisArg, this[i], i, this)) {
      return true;
    }
  }
  return false;
};
```
**Dry Run**:
```js
const arr = [1, 3, 5];
arr.mySome(x => x % 2 === 0); // false
```

---

### 13. `Array.prototype.myFind`
```js
Array.prototype.myFind = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    if (callback.call(thisArg, this[i], i, this)) return this[i];
  }
  return undefined;
};
```
**Dry Run**:
```js
const arr = [5, 12, 8];
arr.myFind(x => x > 10); // 12
```

---

### 14. `Array.prototype.myFindIndex`
```js
Array.prototype.myFindIndex = function(callback, thisArg) {
  for (let i = 0; i < this.length; i++) {
    if (callback.call(thisArg, this[i], i, this)) return i;
  }
  return -1;
};
```
**Dry Run**:
```js
const arr = [5, 12, 8];
arr.myFindIndex(x => x > 10); // 1
```

---

### 15. `Array.prototype.myFill`
```js
Array.prototype.myFill = function(value, start = 0, end = this.length) {
  if (start < 0) start = this.length + start;
  if (end < 0) end = this.length + end;
  for (let i = start; i < end; i++) {
    this[i] = value;
  }
  return this;
};
```
**Dry Run**:
```js
const arr = [1, 2, 3, 4];
arr.myFill(0, 1, 3); // [1, 0, 0, 4]
```

---

## ⏭ Next: Promise Methods...

