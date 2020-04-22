## Handshakes Problem

### Problem
You are hosting a party, and the only rule of the party is that when a guest arrives, that guest **must** shake hands with everyone currently at the party. Given an array of arrays with start time and end time per guest, how would you count the total number of handshakes?

*If a guest arrives at the same time that another guest is leaving, this is included as a handshake.*

### Example
``` js
const party = [[5,6], [2,8], [1,2], [30, 40], [1,2]];
numHandshakes(party) // returns 4:
/*
Guest 0 overlaps with guest 1 => 1 handshake
Guest 1 overlaps with guest 2 and guest 4 => 2 handshakes
Guest 2 overlaps with guest 4 => 1 handshake
*/
```

---

### Brute Force Approach: `O(n^2)` time and `O(1)` space

An initial approach is validate and track every possible handshake combination. How can I *guarantee* the number of handshakes? Given the array as is, you would start with the first guest in the array(`party[0]`), and ensure that their party time and the other guest's party time *overlap*. How can you guarantee that? If we had a timeline and mapped the first start-end time to the second, you would see that `[5,6]` is completely within `[2,8]`. While at first it may seem that you have to consider *every* possible combination, they flatten out to 2 major concerns:

1. Is the start time of the 1st guest within the bounds (`>=` and `<=`) of the 2nd guest?
2. Otherwise, is the start time of the 2nd guest within the bounds of the 1st guest?

The opposite consideration of if the 1st guest's start time is before the range, but the end time is within the range is the same as the 2nd concern listed, and vice-versa.

This ends up being an `n^2` operation, as every possible combination becomes a matrix of `n * n` possibilities, although some are repeated.

TIPS FOR BRUTE FORCE SOLUTION:
- How do you GUARANTEE the final number?
- Consider all possible scenarios for overlap, and remove excess ones as necessary. Also beware of incorrect operators. (What if you had 2 guests at the same time? What if you guests didn't overlap at all? What if it's **not** ordered?)

---

### Optimized Approach: `O(nlogn)` time and `O(n)` space

We can do better! (And yes, you see `nlogn` and we are about to sort some arrays!)

Let's flip this question on its head. Previously, we contained ourselves to the limit of using information in the *past*, we took the array as is and went through all possibilities. What if we made an actual *timeline* and ... do it live?

Consider the idea of being a counter at the door as people are entering and leaving the party.
What are your two options?
1. A guest arrives.
2. A guest leaves.

What information do you need about the party at any given time when a guest arrives or leaves?
1. Current number of handshakes.
2. Current number of guests.

- **When the first guest arrives, what do you know about your end result?** There were 0 handshakes and 0 guests.
- **When the second guest arrives, what do you know?** There *may* be 1 guest, but we don't know for sure - unless we check whether or not someone had already left. If the guest is still there, we add a handshake and a guest. If the guest is **not** there, we remove a guest, do nothing to the number of handshakes, and add the new guest.

- **What about a guest's end time?** Having a timeline of start times allows us to keep track of guests who have come in. And in between any 2 consecutive guests' start times, no one else would have entered. This is valuable because it does not change the number of handshakes. This information is not relevant until we add a new guest.

- **Does it matter when 1 guest arrives or leaves?** The total number of people matter, but each individual leaving does not, and this decouples start times from end times. (Something about being stronger as a bundle of sticks than 1 stick)

This now allows the 1 array to break up into two: start time and end time, with a loop through the start time array and removing guests as necessary in a while loop for the end time array. This ends up as an `O(nlogn)` operation due to sorting the arrays, and subsequent `n` time operations for the loop.

TIPS FOR OPTIMIZED SOLUTION:
1. Do you need to keep the guest arrays coupled?
2. What could you do if you had another option? What if you sorted the array?
3. What if you were to track time instead of guests? What other variable would you need to help with that?

---

### Solution

#### Brute Force: Iterative via nested `for` loops
```js
function numHandshakesIterative(party) {
  let numHandshakes = 0;
  // first `for` loop to loop through the party
  for (let i = 0; i < party.length; i++) {
    // second `for` loop to search through each successive guest
    let guestA = party[i];  // an array of [startTime, endTime]
    for (let j = i + 1; j < party.length; j++) {
      let guestB = party[j];
      // the conditions as stated in the Approach
      if ((guestA[0] >= guestB[0] && guestA[0] <= guestB[1]) || (guestB[0] >= guestA[0] && guestB[0] <= guestA[1])) numHandshakes++;
    }
  }
  return numHandshakes;
}
```
#### Optimized Approach: Track time instead of guests!
```js
function numHandshakes(party) {
  let numHandshakes = 0, numGuests = 0, endIndex = 0, endTimes = [];
  // sort by increasing start time
  party.sort((a, b) => a[0] - b[0])
  // add the end times to a new array
  for (let i = 0; i < party.length; i++) {
    endTimes.push(party[i][1])
  }
  // sort by increasing end time
  endTimes.sort((a,b) => a - b)

  // loop through the starting times
  for (let i = 0; i < party.length; i++) {
    let currPartyTime = party[i][0];
    // remove guests if they have left before the current start time
    while (endTimes[endIndex] < currPartyTime) {
      numGuests--;
      endIndex++;
    }
    numHandshakes+= numGuests;
    numGuests++;
  }
  return numHandshakes;
}
```
