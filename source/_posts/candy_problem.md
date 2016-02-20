title: Candy Problem from LeetCode
date: 2016-02-20
tag: CANDY LEETCODE
---

The [candy problem](https://leetcode.com/problems/candy/) supposes that there are N children standing in a line and each child is assigned a rating value (integer type). You are giving candies to these children subjected to the following requirements:

1. Each child must have at least one candy.
2. Children with a higher rating get more candies than their neighbors.

What is the minimum candies you must give?

## Think about it

The first instinct is 

1. Traversing all children from left to right to find the first local minimum rating value.    
2. Going backwards to give values in ascending order until find the descending rating value. Then going forwords to gives values in ascending order as well until find the descending one.
3. Repeat 1 to 2 until all children visited.

The worse case is that the entire rating array is in descending order, when the time complexity is O(N^2).

## The trap of it

In the algorithm above, lots of cases should be considered, where the equal neighbours, for example, is a big trouble maker. Debugging the algorithm is really hard when the amount of test cases is really large. Therefore, a more elegant and concise solution should be raised.

## Rethink about it

The idea to solve this problem is to divide the entire process into two subprocedures, one is for ascending order processing and the other is for the descending order. The following implementation is quoted from [LeetCode Discuss](https://leetcode.com/discuss/16463/a-simple-solution).

```c++
class Solution {
public:
    int candy(vector<int>& ratings) {
        int size = ratings.size();
        if(size <= 1) return size;
        vector<int> num(size, 1);
        // forward
        for(int i = 1; i < size; i++)
        {
            if(ratings[i] > ratings[i-1]) num[i] = num[i-1] + 1;
        }
        // backward
        for(int i = size-1; i > 0 ; i--)
        {
            if(ratings[i-1] > ratings[i]) num[i-1] = max(num[i] + 1, num[i-1]);
        }
        int sum = 0;
        for(int i : num) sum += i;
        return sum;
    }
};
```