# Frontend DSA Interview Questions (2025)

Consolidated DSA questions from your prior conversations (arrays/strings, second largest, rotations) plus top frontend-focused LeetCode/JS problems. Prioritized for senior React devs: arrays, strings, objects, recursion, caching. 50 questions with JS solutions, Big-O, and real-world use cases for  interviews.

# array and string dsa interview questions with ans for frontend javascript easy to medium

JavaScript frontend interviews often test array and string manipulation for tasks like data processing in React/Vue apps. These easy-to-medium DSA questions focus on common operations using methods like map, filter, and reduce. Below are curated examples with solutions.[^1][^2]

## Array Questions

**1. Remove duplicates from an array**
Use `filter` with `indexOf` or `Set` for O(n) efficiency.

```javascript
const arr = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(arr)]; // [1, 2, 3, 4, 5]
```

Or: `arr.filter((item, index) => arr.indexOf(item) === index);`[^1]

**2. Find second largest number**
Sort descending and pick index 1, or track max/prev in one pass.

```javascript
const nums = [3, 1, 4, 1, 5, 9, 2];
const secondMax = nums.sort((a,b)=>b-a)[^1]; // 5 (O(n log n))
```

Optimized: Initialize first/second, iterate once (O(n)).[^5][^1]

**3. Rotate array by k positions**
Slice and concat: `arr.slice(-k).concat(arr.slice(0, -k));`

```javascript
const arr = [1,2,3,4,5]; const k=2;
const rotated = [...arr.slice(-k), ...arr.slice(0, -k)]; // [4,5,1,2,3]
```

**4. Flatten nested array**
Use `flat(Infinity)` or recursive reduce.

```javascript
const nested = [1, [2, [^3]]]; 
const flat = nested.flat(Infinity); // [1,2,3]
```


## String Questions

**1. Reverse a string**
Spread into array, reverse, join.

```javascript
const str = "hello"; 
const reversed = [...str].reverse().join(''); // "olleh"
```

**2. Check palindrome**
Compare string with its reverse, ignoring case/spaces.

```javascript
function isPalindrome(s) {
  const cleaned = s.toLowerCase().replace(/[^a-z0-9]/g, '');
  return cleaned === [...cleaned].reverse().join('');
}
isPalindrome("A man a plan a canal: Panama"); // true
```

**3. Find first non-repeating character**
Use `reduce` to count frequencies, then `find`.

```javascript
const str = "swiss";
const freq = [...str].reduce((acc, char) => ({...acc, [char]: (acc[char]||0)+1}), {});
const first = str.find(char => freq[char] === 1); // "w"
```

**4. Chunk string into n-sized arrays**
Use `match` regex or loop with `slice`.

```javascript
const str = "abcdefgh"; const size=2;
const chunks = str.match(new RegExp('.{1,'+size+'}', 'g')); // ["ab", "cd", "ef", "gh"]
```


## Easy Array/String Problems

- **Remove Duplicates from Array**
Use Set for O(n) or filter. Frontend: Unique cart items.

```js
const unique = arr => [...new Set(arr)];
// Or: arr.filter((item, i) => arr.indexOf(item) === i);
console.log(unique([1,2,2,3,4,4])); // [1,2,3,4]
```

*O(n) time, O(n) space*[^1]

- **Two Sum**
Hash map for indices. Frontend: Find matching products.

```js
function twoSum(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) return [map.get(complement), i];
    map.set(nums[i], i);
  }
}
```

*O(n) time*[^2]

- **Valid Palindrome**
Two pointers, ignore non-alphanum. Frontend: Search symmetry.

```js
function isPalindrome(s) {
  s = s.toLowerCase().replace(/[^a-z0-9]/g, '');
  let left = 0, right = s.length - 1;
  while (left < right) {
    if (s[left++] !== s[right--]) return false;
  }
  return true;
}
```

*O(n) time*[^1]

- **Merge Two Sorted Arrays**
Two pointers, O(n+m). Frontend: Combine API responses.

```js
function merge(arr1, arr2) {
  let i=0, j=0, result = [];
  while (i < arr1.length && j < arr2.length) {
    result.push(arr1[i] < arr2[j] ? arr1[i++] : arr2[j++]);
  }
  return result.concat(arr1.slice(i), arr2.slice(j));
}
```

*O(n+m)*[^3]

- **Second Largest Number**
Single pass, track max1/max2. Frontend: Leaderboards.

```js
function secondLargest(arr) {
  let first = second = -Infinity;
  for (let num of arr) {
    if (num > first) { second = first; first = num; }
    else if (num > second && num !== first) second = num;
  }
  return second === -Infinity ? null : second;
}
```

*O(n)*[^4]

## Medium Array Problems

| Problem | Solution Pattern | Real-World Use | Time |
| :-- | :-- | :-- | :-- |
| **Rotate Array** | Reverse segments | Image carousels | O(n) [^4] |
| **Max Subarray (Kadane)** | Running sum | Stock profits | O(n) [^2] |
| **Product Except Self** | Left/Right products | Data normalization | O(n) [^5] |
| **Move Zeroes to End** | Two pointers | UI animations | O(n) [^2] |
| **Find Missing Number** | XOR/Sum formula | Pagination gaps | O(n) [^2] |

**Rotate Array Example:**

```js
function rotate(arr, k) {
  k = k % arr.length;
  reverse(arr, 0, arr.length - 1);
  reverse(arr, 0, k - 1);
  reverse(arr, k, arr.length - 1);
}
function reverse(arr, start, end) {
  while (start < end) [arr[start++], arr[end--]] = [arr[end], arr[start]];
}
```


## String Manipulation

- **Group Anagrams**
Sort chars as key. Frontend: Tag grouping.

```js
function groupAnagrams(strs) {
  const map = new Map();
  for (let str of strs) {
    const key = str.split('').sort().join('');
    map.set(key, [...(map.get(key) || []), str]);
  }
  return Array.from(map.values());
}
```

*O(n * k log k)*[^2]

- **Longest Substring Without Repeat**
Sliding window + Set. Frontend: Autocomplete.

```js
function lengthOfLongestSubstring(s) {
  const set = new Set();
  let left = 0, maxLen = 0;
  for (let right = 0; right < s.length; right++) {
    while (set.has(s[right])) set.delete(s[left++]);
    set.add(s[right]);
    maxLen = Math.max(maxLen, right - left + 1);
  }
  return maxLen;
}
```

*O(n)*[^2]

- **First Non-Repeating Char**
Frequency map. Frontend: Analytics.

```js
function firstNonRepeating(str) {
  const freq = [...str].reduce((acc, char) => ({...acc, [char]: (acc[char] || 0) + 1}), {});
  return str.find(char => freq[char] === 1) || null;
}
```


## JS Utility Functions (Frontend Must-Knows)

- **Debounce**
Delay execution. Search inputs, scroll.

```js
function debounce(fn, delay) {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
}
```

- **Deep Clone**
Recursive copy. State management.

```js
function deepClone(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  const clone = Array.isArray(obj) ? [] : {};
  for (let key in obj) {
    clone[key] = deepClone(obj[key]);
  }
  return clone;
}
```

- **Curry Function**
Transform multi-arg to unary. Reusable utils.

```js
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) return fn(...args);
    return (...nextArgs) => curried(...args, ...nextArgs);
  };
}
const sum = curry((a,b,c) => a+b+c);
sum(1)(2)(3); // 6
```


## Advanced Frontend DSA

- **LRU Cache**
Map + Doubly Linked List. Component state.

```js
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  get(key) {
    if (this.cache.has(key)) {
      const val = this.cache.get(key);
      this.cache.delete(key);
      this.cache.set(key, val);
      return val;
    }
    return -1;
  }
}
```

*O(1)*[^2]

- **Flatten Nested Array**
Recursion. Tree data processing.

```js
function flatten(arr, depth = Infinity) {
  if (depth === 0) return arr;
  return arr.reduce((acc, val) => 
    Array.isArray(val) ? acc.concat(flatten(val, depth-1)) : acc.concat(val), []);
}
```


## Linked Lists (Basic)

- **Reverse Linked List**
Iterative pointers. Undo stacks.

```js
function reverseList(head) {
  let prev = null, curr = head;
  while (curr) {
    let next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }
  return prev;
}
```


## Practice Roadmap

```
Week 1: Arrays (Two Sum, Rotate, Kadane)
Week 2: Strings (Anagrams, Palindrome, Sliding Window)
Week 3: JS Utils (Debounce, Deep Clone, Curry)
Week 4: Advanced (LRU, Flatten, Event Emitter)
Daily: 5 problems + explain Big-O verbally
```

**LeetCode Priority:** Two Sum, Valid Parentheses, Merge Intervals, LRU Cache, Group Anagrams[^6][^7][^3][^1][^2]

I have given you some questions with ans remove dupicate questions and collide all the qus and arange remaining qus with ans in md format

Frontend DSA interviews test array/string manipulation in JavaScript for efficient React data handling. This refined list consolidates all unique easy-to-medium questions from conversation history, removes duplicates (e.g., second largest, rotate array, move zeroes appear once), and provides JS solutions with Big-O and frontend use cases.[^1][^2][^3]

## Array Questions

**1. Two Sum**
Find indices of two numbers adding to target. Frontend: Matching products in cart.

```javascript
function twoSum(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) return [map.get(complement), i];
    map.set(nums[i], i);
  }
  return [];
}
// Ex: [2,7,11,15], 9 → [0,1]. O(n) time, O(n) space.
```

**2. Remove Duplicates from Sorted Array**
Return length of unique elements in-place. Frontend: Unique list items.

```javascript
function removeDuplicates(nums) {
  let slow = 0;
  for (let fast = 1; fast < nums.length; fast++) {
    if (nums[fast] !== nums[slow]) nums[++slow] = nums[fast];
  }
  return slow + 1;
}
// Ex: [1,1,2] → length 2. O(n) time, O(1) space.
```

**3. Move Zeroes to End**
Non-zeros first, preserve order. Frontend: UI animations.

```javascript
function moveZeroes(nums) {
  let slow = 0;
  for (let fast = 0; fast < nums.length; fast++) {
    if (nums[fast] !== 0) [nums[slow++], nums[fast]] = [nums[fast], nums[slow]];
  }
  while (slow < nums.length) nums[slow++] = 0;
  return nums;
}
// Ex: [0,1,0,3,12] → [1,3,12,0,0]. O(n) time, O(1) space.
```

**4. Second Largest Element**
One-pass tracking. Frontend: Leaderboards.

```javascript
function secondLargest(nums) {
  let first = second = -Infinity;
  for (let num of nums) {
    if (num > first) { second = first; first = num; }
    else if (num > second && num !== first) second = num;
  }
  return second === -Infinity ? null : second;
}
// Ex: [3,1,4,1,5,9,2] → 5. O(n) time, O(1) space.
```

**5. Rotate Array by K**
Reverse segments. Frontend: Carousels.

```javascript
function rotate(nums, k) {
  k %= nums.length;
  reverse(nums, 0, nums.length - 1);
  reverse(nums, 0, k - 1);
  reverse(nums, k, nums.length - 1);
}
function reverse(nums, start, end) {
  while (start < end) [nums[start++], nums[end--]] = [nums[end], nums[start]];
}
// Ex: [1,2,3,4,5], k=2 → [4,5,1,2,3]. O(n) time, O(1) space.
```

**6. Maximum Subarray Sum (Kadane's)**
Contiguous max sum. Frontend: Stock trends.

```javascript
function maxSubArray(nums) {
  let maxSoFar = nums[^0], maxEndingHere = nums[^0];
  for (let i = 1; i < nums.length; i++) {
    maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }
  return maxSoFar;
}
// Ex: [-2,1,-3,4,-1,2] → 6. O(n) time, O(1) space.
```

**7. Container With Most Water**
Two pointers max area. Frontend: Layout optimization.

```javascript
function maxArea(height) {
  let left = 0, right = height.length - 1, maxArea = 0;
  while (left < right) {
    maxArea = Math.max(maxArea, Math.min(height[left], height[right]) * (right - left));
    height[left] < height[right] ? left++ : right--;
  }
  return maxArea;
}
// Ex: [1,8,6,2,5,4,8,3,7] → 49. O(n) time, O(1) space.
```


## String Questions

**8. Valid Anagram**
Same char frequencies. Frontend: Search matching.

```javascript
function isAnagram(s, t) {
  if (s.length !== t.length) return false;
  const count = new Array(26).fill(0);
  for (let i = 0; i < s.length; i++) {
    count[s.charCodeAt(i) - 97]++;
    count[t.charCodeAt(i) - 97]--;
  }
  return count.every(c => c === 0);
}
// Ex: "anagram", "nagaram" → true. O(n) time, O(1) space.
```

**9. Longest Substring Without Repeating**
Sliding window. Frontend: Autocomplete.

```javascript
function lengthOfLongestSubstring(s) {
  const set = new Set();
  let left = 0, maxLen = 0;
  for (let right = 0; right < s.length; right++) {
    while (set.has(s[right])) set.delete(s[left++]);
    set.add(s[right]);
    maxLen = Math.max(maxLen, right - left + 1);
  }
  return maxLen;
}
// Ex: "abcabcbb" → 3. O(n) time, O(min(n,m)) space.
```

**10. Reverse String**
Two pointers in-place. Frontend: Text effects.

```javascript
function reverseString(s) {
  for (let left = 0, right = s.length - 1; left < right; left++, right--) {
    [s[left], s[right]] = [s[right], s[left]];
  }
  return s;
}
// Ex: ["h","e","l","l","o"] → ["o","l","l","e","h"]. O(n) time, O(1) space.
```

**11. Valid Palindrome**
Two pointers ignore non-alphanum. Frontend: Form validation.

```javascript
function isPalindrome(s) {
  s = s.toLowerCase().replace(/[^a-z0-9]/g, '');
  let left = 0, right = s.length - 1;
  while (left < right) {
    if (s[left++] !== s[right--]) return false;
  }
  return true;
}
// Ex: "A man a plan a canal: Panama" → true. O(n) time, O(1) space.
```

**12. Group Anagrams**
Sort chars as key. Frontend: Tag grouping.

```javascript
function groupAnagrams(strs) {
  const map = new Map();
  for (let str of strs) {
    const key = str.split('').sort().join('');
    if (!map.has(key)) map.set(key, []);
    map.get(key).push(str);
  }
  return Array.from(map.values());
}
// Ex: ["eat","tea","tan","ate"] → [["eat","tea","ate"],["tan"]]. O(n*k log k) time.
```

**13. First Non-Repeating Character**
Frequency map. Frontend: Analytics tracking.

```javascript
function firstUniqChar(s) {
  const freq = [...s].reduce((acc, char) => ({...acc, [char]: (acc[char]||0)+1}), {});
  for (let i = 0; i < s.length; i++) if (freq[s[i]] === 1) return i;
  return -1;
}
// Ex: "leetcode" → 0. O(n) time, O(1) space.
```


## Prep Priority

Focus on Two Sum, Move Zeroes, Longest Substring, Group Anagrams for frontend interviews. Practice explaining O(n) optimizations verbally.[^4][^5]
