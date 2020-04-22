class: center, middle

# Greedy Algorithm

---

class: center, middle

# Prompt

Given a string, reduce the string by removing 3 or more consecutive identical characters. You should greedily remove characters from left to right.

Follow-up: What if you need to find the shortest string after removal?

---
class: center, middle

# Examples

**Example 1:**

```
Input: "aaabbbc"
Output: "c"
Explanation:
1. Remove 3 'a': "aaabbbc" => "bbbc"
2. Remove 3 'b': "bbbc" => "c"
```
**Example 2:**

```
Input: "aabbbacd"
Output: "cd"
Explanation:
1. Remove 3 'b': "aabbbacd" => "aaacd"
2. Remove 3 'a': "aaacd" => "cd"
```
**Example 3:**

```
Input: "aabbccddeeedcba"
Output: ""
Explanation:
1. Remove 3 'e': "aabbccddeeedcba" => "aabbccdddcba"
2. Remove 3 'd': "aabbccdddcba" => "aabbcccba"
3. Remove 3 'c': "aabbcccba" => "aabbba"
4. Remove 3 'b': "aabbba" => "aaa"
5. Remove 3 'a': "aaa" => ""
```
**Example 4:**

```
Input: "aaabbbacd"
Output: "acd"
Explanation:
1. Remove 3 'a': "aaabbbacd" => "bbbacd"
2. Remove 3 'b': "bbbacd" => "acd"
```

---

# Solution

First initalize a stack. Then loop over the characters of the string. If the character isn't the same as two positions ago in the stack then add the character to the stack. Otherwise remove the two previous characters from the stack and store the value of the recently removed character.  By store the recentlyPop character we are able to greedily loop through the string and avoid adding more than 3+ consecutive identical letters to our stack. Once we have finished looping through our string we return our stack with the join method to output a new string. This ends up being `O(n)` time complexity where `n` is size of the given string, and `O(n)` space complexity where `n` is size of the created stack.

```js
const candyCrush = (str) =>{

  if(!str.length) return ""

  let stack=[]
  let recentlyPop;

  for (const char of str) {
    if(char === recentlyPop) continue
    else if(char !== stack[stack.length-2]) stack.push(char)
    else{
      recentlyPop = stack.pop()
      stack.pop()
    }
  }
  return stack.join("");
}
```
---

**Follow-up example:**

```
Input: "aaabbbacd"
Output: "cd"
Explanation:
1. Remove 3 'b': "aaabbbacd" => "aaaacd"
2. Remove 4 'a': "aaaacd" => "cd"
```

# Follow-up Solution



```js
const candyCrushShortest = str => {
  const findSolutions = str => {
    if (str === "" || str.length === 1) {
      return [str];
    }
    let groups = [];
    let start = 0;
    let count = 0;
    let char = "";
    for (let i = 0; i < str.length; ++i) {
      if (str[i] != char) {
        if (count >= 3) {
          groups.push(str.slice(0, start) + str.slice(start + count));
        }
        char = str[i];
        start = i;
        count = 1;
      } else {
        ++count;
      }
    }
    for (let i = 0; i < groups.length; ++i) {
      if (groups[i].length > 1) {
        groups = groups.concat(findSolutions(groups[i]));
      }
    }
    return groups;
  };
  return findSolutions(str).sort((a, b) => a.length - b.length)[0];
};
```
