# [Grokking Coding Interview Patterns in C#](https://www.educative.io/courses/grokking-coding-interview-in-csharp)

## Two Pointers
Used on sequential data structures like arrays or linkedlists. it involves maintaining two pointers that traverse the data structure in a coordinated manner, typically starting from different positions or moving in opposite directions. 

These pointers dynamically adjust based on specific conditions or criteria, allowing for the efficient exploration of the data and enabling solutions with optimal time and space complexity.

Whenever there’s a requirement to find two data elements in an array that satisfy a certain condition, the two pointers pattern should be the first strategy to come to mind.

### Is Valid Palindrome
```
using System;

public class Solution {
    public static bool IsPalindrome(string s) {

        // Replace this placeholder return statement with your code
        //abcca
        int start = 0;
        int end = s.Length - 1;
        
        while(start <= end)
        {
          if(s[start] != s[end])
          {
            return false;
          }
          
          start++;
          end--;
        }
        
        
        return true;
    }
}

```

### 3Sum
```
using System;

public class Solution{
    public static bool FindSumOfThree(int[] nums, int target) {
        
        //[-240,-855,-765] , -466
        // Replace this placeholder return statement with your code
        Array.Sort(nums);
        
        for(int current = 0; current < nums.Length-1; current++)
        {
            int low = current+1;
            int high = nums.Length-1;
            
            while(low < high)
            {
              if(nums[current] + nums[low] + nums[high] == target)
              {
                return true;
              }
              
              else
              {
                if(nums[current] + nums[low] + nums[high] < target)
                {
                  low++;
                }
                if(nums[current] + nums[low] + nums[high] > target)
                {
                  high--;
                }
              }
            }
        }
        
        return false;
    }
}

```

### Remove Nth Node from End of List
```


```

## Fast and Slow Pointers

## Sliding Window

## Merge Intervals

## In-Place Manipulation of a Linked List

## Two Heaps

## K-way merge

## Top K Elements

## Modified Binary Search

## Subsets

## Greedy Techniques

## Backtracking

## Dynamic Programming

## Cyclic Sort

## Topological Sort

## Matrices

## Stacks

## Graphs

## Tree Depth-First Search

## Tree Breadth-First Search

## Trie

## Hash Maps

## Knowing What to Track

## Union Find

## Custom Data Structures

## Bitwise Manipulation

## Challenge Yourself

## Conclusion
