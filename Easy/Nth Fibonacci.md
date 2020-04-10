## **Nth Fibonacci**


### **PROMPT**
The Fibonacci sequence is defined as follows: the first number of the sequence is 0, the second number is 1, and the nth number
is the sum of the (n-1)th and (n-2)th numbers. Write a function that takes in an integer n and returns the nth Fibonacci number.

**test cases**

* console.log(nthFib(6)) //5 ->(0,1,1,2,3,5)
* console.log(nthFib(3)) //1 ->(0,1,1)
* console.log(nthFib(8)) //13 ->(0,1,1,2,3,5,8,13)
* console.log(nthFib(1)) //0 ->(0)


## **Solutions**

**naive solution**
```
function nthFib(n) {
  if(n === 1) return 0
  if(n === 2) return 1
  return nthFib(n-2) + nthFib(n-1)
}
// O(2^n) time | O(n) space
```

**partially optimized solution**
```
function nthFib(n, memoize = {1: 0, 2: 1}) {
  if(n in memoize) {
    return memoize[n]
  } else {
    memoize[n] = nthFib(n - 1, memoize) + nthFib(n - 2, memoize)
    return memoize[n]
  }
}
// O(n) time | O(n) space
// Potentially better time complexity as our memoization stores more sequence values.
```

**optimal solution**
```
function nthFib(n) {
  const lastTwo = [0, 1]
  let counter = 3
  while(counter <= n) {
    const nextFib = lastTwo[0] + lastTwo[1]
    lastTwo[0] = lastTwo[1]
    lastTwo[1] = nextFib
    counter++
  }
  return n > 1 ? lastTwo[1] : lastTwo[0]
}
// O(n) time | 0(1) space
```


### **Interviewer Prompts:**
For the naive and partially optimized solutions, guide them toward a recursive approach if they are struggling to realize that
recursion is very useful here. If they make it all the way to the optimal solution, you may be able to help them by hinting
that the only values that are every really needed on any given nthFib function call are n - the fibonacci sequence number
as well as the values of the previous two sequence numbers. With that knowledge they might be able to start dynamically building
the solution with an array and value swapping.
