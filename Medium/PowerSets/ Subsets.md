class: center middle

## Powerset

---

## Interviewer Prompt

Given a set of distinct integers, nums, return all possible subsets (the power set).
The solution set must not contain duplicate subsets.

---

## Example

Given an input of nums = [1,2,3]
Output will be
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

---

## Solution Code (Binary Mapping)

```javascript
let nums = [1,2,3]

const subset = (numArr) => {
  // initalize powerset array
  let subset = []

  //edge case
  if(numArr.length ===0) return subset

  // find total number of sets that power set will contain
  let total = Math.pow(2,numArr.length)

  // loop through each value from 0 to 2^n
  for(let i=0; i<total; i++){

    let tempSet = []

    //convert integer to binary
    let num = i.toString(2)

    //pad binary number so "1" becomes "001" etc
    while(num.length < numArr.length) num = `0${num}`

    // build the set that matches the 1's in the binary number
    for(let j = 0; j<num.length; j++){
      if(num[j] === "1") tempSet.push(numArr[j])
    }

    // add this set to the final power set
    subset.push(tempSet)
  }
  console.log(subset(nums))
```

---

## Solution Code (Backtracking Method)

```javascript
let nums = [1,2,3]

const subset = (numArr) => {
  // initalize powerset array
  let subset = []

  //edge case
  if(numArr.length ===0) return subset

  subset.push([])

  numArr.forEach(elem =>{
    let subLength = subset.length
    let subIndex = 0

    while (subIndex<subLength){
      // create a temp subArry
      let temp = subset[subIndex].slice(0)

      // push current elem into subArry
      temp.push(elem)

      // add subArry into Powerset Array
      subset.push(temp)

      subIndex++
    }
  })
  return subset
}

console.log(subset(nums))
```

---

## Summary

Big O

- Time Complexity: O(2^n) because there are 2n possible sets in a power set for an input set with n elements
- Space Complexity: O(2^n)
