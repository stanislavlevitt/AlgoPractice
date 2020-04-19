class: center middle
## **Single Number**

--
### Interviewer Prompt
Given a non-empty array of integers, every element appears twice except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

---

## Example

* console.log(pairSum([2,2,1] ) //1
* console.log(pairSum([4,1,2,1,2]) //4


## Solution Code (Double forLoop)
```
const singleNumber = (nums) => {
    nums.sort((a,b) => a-b)

    let count = 0
    let number = nums[0]

    for(let i=1; i<nums.length; i++){
        if(nums[i] === number && count ===0){
            count ++
        }
        else if (nums[i] !== number && count ===1){
            number = nums[i]
            count =0
        }
    }
    return number

};
```
Time Complexcity O(n + n)


## Solution Code (Refractor)
```
const singleNumber = (nums) {
    nums.sort((a,b) => a-b)

    for (let i = 0; i < nums.length; i += 2) {
        if (nums[i] != nums[i + 1]) {
            return nums[i];
        }
    }
};
```
