title: Single Number Problems
date: 2016-02-25
tag: LEETCODE
----
## QUESTIONS
Three single number problems are listed in LeetCode in total and quoted as follows:

>1. "Given an array of integers, every element appears **twice** except for one. Find that single one." [Q1]
2. "Given an array of integers, every element appears **three times** except for one. Find that single one." [Q2]
3. "Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once." [Q3]

The basic requirements includes both linear time complexity and constant space complexity. Therefore, we can neither count all numbers nor sort the array, for counting numbers costs _O(N)_ space complexity and sorting costs _O(N*logN)_ time complexity at least.
## SOLUTIONS for Q1 and Q2
### Straightforward Solution
The straightforward thought might be the bit operations. An integer is stored in 32 or 64 bits. We take down the number of _1_ of every bit of all elements. Then for every bit, the number of _1_ can be divided by _k_ if each element appears _k_ times. This method works well for Q1 and Q2.
```
class Solution {  
public:  
    int singleNumber(vector<int> nums) {
        vector<int> bitnum(32, 0);  
        int res = 0;
        for(int i = 0; i < 32; i++){  
            for(int j = 0; j < n; j++){  
                bitnum[i] += (nums[j] >> i) & 1;  
            }  
            res |= (bitnum[i] % 2) << i; // here k equals 2 or 3
        }  
        return res;
    }  
}; 
```
### Tricky Solution
The second thought could be a more tricky one. As ___A xor A xor B = B___, we could use ___xor___ for all elements, as long as the number of _A_ is even and the number of _B_ is odd. Therefore, it only works for Q2.
```
class Solution {
public:
    int singleNumber(vector<int> nums) {
        int res = 0;
        for(auto i : nums) res ^= i;
        return res;
    }
};
```
### Generalized Solution
The third thought is to generalize the first and second method to works well for both Q1 and Q2. As we can see, the main idea above is to return zero when '_1_' occurs certain times _k_. (Tip: here we just focus on the number of '_1_' because '_0_' always be zero no matter how many '_0_' appears). For example, when _k_ equals 2, the transformation will be 
```
bit1: 0 ----> 1 ----> 0 ......
```
while _k_ equals 3, we need one more bit to record (3 < 2^2). The transformation will show that bits all return to zero after _1_ appears every 2 times.
```
bit1: 0 ----> 0 ----> 1 ----> 0 ----> 0 ......
bit2: 0 ----> 1 ----> 0 ----> 0 ----> 1 ......
```
Moreover, if _k_ equals 5 (2^2 < 5 < 2^3), then three bits are needed for recording and the transformation following shows that it returns zero after _1_ appears every 5 times.
```
bit1: 0 ----> 0 ----> 0 ----> 0 ----> 1 ----> 0 ......
bit2: 0 ----> 0 ----> 1 ----> 1 ----> 0 ----> 0 ......
bit3: 0 ----> 1 ----> 0 ----> 1 ----> 0 ----> 0 ......
```
It's the **boolean algebra**. 
First, we should use proper number of bits to record the times that '_1_' shows up; 
Second, we should find a set of boolean algebra formulas to represent these transforms. We should notice that the formulas set could be writen in different ways.
Finally, we could just ignore the duplicated elements and find out the target one.
```
// Q2 could be solve as follows
public int singleNumber(vector<int> nums) {
    int bit1 = 0, bit2 = 0;
    for(int i = 0; i < nums.size(); i++){
        // here use two bits to represent the times '1' shows up
        bit1 = (bit1 ^ nums[i]) & ~bit2;
        bit2 = (bit2 ^ nums[i]) & ~bit1;
    }
    return bit1; // the element come up only once
}
```
## SOLUTION for Q3
The main difference for Q3 is that it has two target numbers. We denote x and y to represent the two target number, then `x^y` could be easily got. There must be a set of bit '_1_' in `x^y`, which means that x differs from y at these bits of '_1_'. One bit is enough to distinguish x and y. Here we use the last '_1_' to mark x and y. Since `(x^y)&(-x^y)` could set '_1_' at the bit where last '_1_' occurs while set '_0_' else. Then, we categorize all numbers into two groups `A` and `B` according to its value on this bit. The xor of group `A` and group `B` will be the two target numbers.

>A^A^B^B^x^y = x^y
A^A^x = x
B^B^y = y
x^y^x = y

```C++
vector<int> singleNumber(vector<int>& nums) {
    int a = 0, b = 0;
    for (int n : nums) a ^= n;
    for (int n : nums) 
        if (n & a & -a) b ^= n;
    return {a ^ b, b};
}
```

**REFERENCE**
https://leetcode.com/problems/single-number/
https://leetcode.com/problems/single-number-ii/
https://leetcode.com/problems/single-number-iii/
https://leetcode.com/discuss/52377/3-lines-ruby-4-lines-c
