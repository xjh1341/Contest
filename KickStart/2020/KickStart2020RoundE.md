## Kick Start 2020 Round E

[link](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ff47)

### A. Longest Arithmetic (4pts, 7pts)

An arithmetic array is an array that contains at least two integers and the differences between consecutive integers are equal. For example, [9, 10], [3, 3, 3], and [9, 7, 5, 3] are arithmetic arrays, while [1, 3, 3, 7], [2, 1, 2], and [1, 2, 4] are not arithmetic arrays.

Sarasvati has an array of **N** non-negative integers. The i-th integer of the array is **Ai**. She wants to choose a contiguous arithmetic subarray from her array that has the maximum length. Please help her to determine the length of the longest contiguous arithmetic subarray.

**Input**

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with a line containing the integer **N**. The second line contains **N** integers. The i-th integer is **Ai**.

**Output**

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the length of the longest contiguous arithmetic subarray.

**Limit**

Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
0 ≤ **Ai** ≤ 109.

**Test Set 1**

2 ≤ **N** ≤ 2000.

**Test Set 2**

2 ≤ **N** ≤ 2 × 105 for at most 10 test cases.
For the remaining cases, 2 ≤ **N** ≤ 2000.

**Sample**

| Input                                                        | Output                                           |
| ------------------------------------------------------------ | ------------------------------------------------ |
| `4 7 10 7 4 6 8 10 11 4 9 7 5 3 9 5 5 4 5 5 5 4 5 6 10 5 4 3 2 1 2 3 4 5 6   ` | `Case #1: 4 Case #2: 4 Case #3: 3 Case #4: 6   ` |

In Sample Case #1, the integers inside the bracket in the following represent the longest contiguous arithmetic subarray: 10 7 [4 6 8 10] 11

In Sample Case #2, the whole array is an arithmetic array, thus the longest contiguous arithmetic subarray is the whole array.

In Sample Case #3, the longest contiguous arithmetic subarray is either [5, 5, 5] (a subarray from the fourth integer to the sixth integer) or [4, 5, 6] (a subarray from the seventh integer to the ninth integer).

In Sample Case #4, the longest contiguous arithmetic subarray is the last six integers.

**Analysis**

Consider a subarray of length K which is an arithmetic subarray, and let the elements of arithmetic subarray be B1, B2, B3, ....., BK. We can say that B2 - B1 = Bi+1 - Bi for 1 ≤ i < K, because consecutive elements of arithmetic sequence should have a common difference.

**Claim 1:** In the given array, consider a subarray starting at index i and ending at index j. Now if this subarray is not arithmetic, there exists some index x such that i ≤ x < j and **A**x+1 - **A**x ≠ **A**i+1-**A**i. All subarrays starting at index i and ending at index y such that x < y ≤ **N**, are not arithmetic because all such subarrays would contain index x such that **A**x+1 - **A**x ≠ **A**i+1-**A**i.

**Test Set 1**

For each element i such that (1 ≤ i < **N**), we consider each subarray starting at index i. Consider subarray(i,j) and start with j = i. Increment j while subarray(i,j) is an arithmetic subarray. For a fixed index i, we do not need to increment j after we find the first index such that subarray(i,j) is not an arithmetic subarray. None of the subarrays with i as a starting point and ending point after the index j will be arithmetic subarrays according to Claim 1. Let the maximum j for index i such that subarray(i,j) is an arithmetic subarray be max_j. We can conclude our approach as follows. Initialise the answer as 0. For each index i, find max_j. Update the answer if max_j - i + 1 is greater than the answer. For each index i, we would traverse O(**N**) elements. Hence, the overall complexity of the solution is O(**N**2).

**Sample Code (C++)**

```cpp
int maxArithmeticSubarray(vector<int> array) {
  int maxLen = 0;
  for(int i = 0; i < array.size() - 1; i++) {
     int j = i;
     int common_difference = array[i+1] - array[i];
     while(j < array.size() - 1 && (array[j + 1] - array[j] == common_difference))
          j++;
     int max_j = j;
     maxLen = max(maxLen, max_j - i + 1);
  }
  return maxLen;
}
```

**Test Set 2 (C++)**

Consider an index i. Now consider all the subarrays (i,j) starting at index i and ending at index j. Start with j = i. Let the maximum index j where the subarray(i,j) is an arithmetic subarray be j = x. Let **A**i+1 - **A**i = D. We can say that **A**y+1 - **A**y = D for all i ≤ y < x. We have 2 cases now.

- Case 1: x = **N**,
  In this case, subarray(i, **N**) is an arithmetic subarray. All subarrays(p, **N**) such that i < p ≤ N, will have shorter length than subarray(i, **N**). Hence, we can discard all subarrays starting with index p.
- Case 2: x ≠ **N**,
  **A**x+1 - **A**x ≠ D. We have already proved that we need not consider j > x for index i as those subarrays will not be arithmetic using Claim 1. Now consider subarrays (k, x + 1) such that ( i+1 ≤ k < x). All these subarrays are not arithmetic because **A**x+1 - **A**x ≠ D whereas Ak+1 - Ak = D. Hence, we can discard all the subarray starting with index k. So, we can now shift the starting index to x.

We can conclude our approach as follows. Initialise the answer as 0. We maintain two pointers, left pointer i and right pointer j. For an index i, we try to find the longest arithmetic subarray starting at index i by incrementing j. Let the maximum j for index i such that subarray(i,j) is an arithmetic subarray be max_j. Update answer if max_j - i + 1 is greater than current answer. And then we shift our left pointer i to the current max_j. We can see that both the pointers visit each element at most once. Hence, the complexity of the solution is O(**N**).

**Code**

```
int maxArithmeticSubarray(vector<int> array) {
  int maxLen = 0;
  for(int i = 0; i < array.size() - 1;) {
     int j = i;
     int common_difference = array[i+1] - array[i];
     while(j < array.size() - 1 && (array[j + 1] - array[j] == common_difference))
          j++;
     int max_j = j;
     maxLen = max(maxLen, max_j - i + 1);
     i = max(i + 1, j);
  }
  return maxLen;
}
```



### High Buildings (6pts, 9pts)

In an unspecified country, Google has an office campus consisting of **N** office buildings in a line, numbered from 1 to **N** from left to right. When represented in meters, the height of each building is an integer between 1 to **N**, inclusive.

Andre and Sule are two Google employees working in this campus. On their lunch break, they wanted to see the skyline of the campus they are working in. Therefore, Andre went to the leftmost point of the campus (to the left of building 1), looking towards the rightmost point of the campus (to the right of building **N**). Similarly, Sule went to the rightmost point of the campus, looking towards the leftmost point of the campus.

To Andre, a building x is visible if and only if there is no building to the left of building x that is strictly higher than building x. Similarly, to Sule, a building x is visible if and only if there is no building to the right of building x that is strictly higher than building x.

Andre learned that there are **A** buildings that are visible to him, while Sule learned that there are **B** buildings that are visible to him. After they regrouped and exchanged information, they also learned that there are **C** buildings that are visible to both of them.

They are wondering about the height of each building. They are giving you the value of **N**, **A**, **B**, and **C** for your information. As their friend, you would like to construct a possible height for each building such that the information learned on the previous paragraph is correct, or indicate that there is no possible height construction that matches the information learned (thus at least one of them must have been mistaken).

**Input**

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each consists of a single line with four integers **N**, **A**, **B**, and **C**: the information given by Andre and Sule.

**Output**

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is `IMPOSSIBLE` if there is no possible height for each building according to the above information, or **N** space-separated integers otherwise. The i-th integer in `y` must be the height of the i-th building (in meters) between 1 to **N**.

**Limits**

Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ **T** ≤ 100.
1 ≤ **C** ≤ **N**.
**C** ≤ **A** ≤ **N**.
**C** ≤ **B** ≤ **N**.

**Test Set 1**

1 ≤ **N** ≤ 5.

**Test Set 2**

1 ≤ **N** ≤ 100.

**Sample**

| Input                          | Output                                                       |
| ------------------------------ | ------------------------------------------------------------ |
| `3 4 1 3 1 4 4 4 3 5 3 3 2   ` | `Case #1: 4 1 3 2 Case #2: IMPOSSIBLE Case #3: 2 1 5 5 3   ` |

In Sample Case #1, the sample output sets the height of each building such that only the first building is visible to Andre, while the first, third, and fourth buildings are visible to Sule. Therefore, only the first building is visible to both Andre and Sule. Note that there exist other correct solutions, such as `4 3 1 2`.

In Sample Case #2, all **N** = 4 buildings are visible to Andre and Sule. Therefore, it is impossible to have **C** ≠ **N** in this case.

In Sample Case #3, the sample output sets the height of each building such that the first, third, and fourth buildings are visible to Andre, while the third, fourth, and fifth buildings are visible to Sule. Therefore, the third and fourth buildings are visible to both Andre and Sule. Note that there exist other correct solutions.