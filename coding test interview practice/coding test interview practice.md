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

### Remove Nth Node from End of Linked List
Linked List contains data and a pointer to the next node
Relearn about the Linked List from the Udemy Course and come back to solve this test

#### Linked List
```
//LinkedListNode.h
#pragma once
struct LinkedListNode
{
  
public:
  LinkedListNode();
  LinkedListNode(int data);

  int data;
  LinkedListNode* next;
};

//LinkedListNode.cpp
#include "LinkedListNode.h"

LinkedListNode::LinkedListNode()
{
  data = 0;
  next = nullptr;
}

LinkedListNode::LinkedListNode(int data)
{
  this->data = data;
  next = nullptr;
}



//LinkedList.h
#include"LinkedListNode.h"
#pragma once
class LinkedList
{
private:
  LinkedListNode* head;

public:
  LinkedList();
  bool Add(LinkedListNode* node);
  bool Remove(const int data);
  LinkedListNode* GetNode(const int data);
  void Display();
};

//LinkedList.cpp
#include "LinkedList.h"
#include <iostream>

using namespace std;

LinkedList::LinkedList()
{
  head = new LinkedListNode();
}

bool LinkedList::Add(LinkedListNode* node)
{
  LinkedListNode* end = head;

  while (end->next != nullptr)
  {
    end = end->next;
  }

  end->next = node;
  return true;
}

bool LinkedList::Remove(const int data)
{
  LinkedListNode* nodeToRemove = GetNode(data);
  LinkedListNode* previous = head;

  if (nodeToRemove == nullptr || nodeToRemove == head)
  {
    return false;
  }

  while (previous->next != nodeToRemove && previous->next != nullptr)
  {
    previous = previous->next;
  }

  previous->next = nodeToRemove->next;
  delete nodeToRemove;
  return true;
}

LinkedListNode* LinkedList::GetNode(const int data)
{
  LinkedListNode* nodeToRemove = nullptr;
  LinkedListNode* end = head;

  while (end->next != nullptr)
  {
    if (end->data == data)
    {
      nodeToRemove = end;
      return nodeToRemove;
    }
    end = end->next;
  }

  return nodeToRemove;
}

void LinkedList::Display()
{
  LinkedListNode* p = head;

  while (p->next != nullptr)
  {
    cout << p->data << "->" << flush;
    p = p->next;
  }

  cout << p->data << "->" << flush;
}


```

#### Remove Nth Node from End of Linked List
```
using System.Collections.Generic;

public class Solution
{
    public static LinkedListNode RemoveNthLastNode(LinkedListNode head, int n)
    {
        //32 -> 78 -> 65 -> 90 -> 12 -> 44. n = 3
        // Replace this placeholder return statement with your code
        LinkedListNode start = head;
        LinkedListNode end = head;
        LinkedListNode nodeToRemove;
        
        //TODO move right n steps forward
        for(int i = 1; i<=n; i++ )
        {
          end = end.next;
        }
        
        if(end == null) return head.next;
        
        //TODO move left and right forward until right-> next is null
        while(end.next != null)
        {
          start = start.next;
          end = end.next;
        }
        
        //TODO left will be the previous of the nth node to Remove. 
        //then left->next = next->next
        nodeToRemove = start.next;
        start.next = nodeToRemove.next;
        
        return head;
    }
}
```

### Sort Colors
Try to solve the Sort Colors problem.
```
using System;

public class Solution {
    public static int[] SortColors (int[] colors) {

        // Write your code here using the 2 pointers pattern with comments teaching me
        //[1, 0, 2, 1, 2, 2] - colors (red - 0 , white - 1, blue - 2)
        int redPointer = 0;
        int bluePointer = colors.Length - 1;
        int current = 0;
        
        while (current <= bluePointer)
        {
          //TODO if current is 0 swap with redpointer and increment current and redpointer
          if(colors[current] == 0)
          {
            Swap(ref colors[current], ref colors[redPointer]);
            current++;
            redPointer++;
          }
          
          else if(colors[current] == 1)
          {
            current++;
          }
          
          //TODO if current is 2 swap with bluePointer and decrement only bluePointer
          else
          {
            Swap(ref colors[current], ref colors[bluePointer]);
            bluePointer--;
          }
        }
        return colors;
    }
    
    private static void Swap(ref int a, ref int b)
    {
      var tempa = a;
      a = b;
      b = tempa;
    }
}
```

### Reverse Words in a String
Try to solve the Reverse Words in a String problem.

My Solution
```
using System;

public class Solution {
    public static string ReverseWords(string sentence) {

        // Replace this placeholder return statement with your code
        // Reverse the words in a sentence
        //split this string based on a namespace
        //create a new string starting from end

        string[] splitString = sentence.Split(' ');
        var reversedWords = "";

        for (int i = splitString.Length - 1; i >= 0; i--)
        {
            if (i == 0 || i == splitString.Length - 1)
            {
                if (splitString[i] == "") continue;
                if (reversedWords.Length > 0) reversedWords += " ";
                reversedWords += splitString[i];
            }

            else
            {
                if (splitString[i] == "") continue;
                if(reversedWords.Length > 0)reversedWords += " ";
                reversedWords += splitString[i];
            }
        }

        return reversedWords;
    }
}
```

Correct Solution
```
using System;

public class Solution 
{
    public static string CorrectSolution(string sentence)
    {
        static void StrRev(ref char[] str, int startRev, int endRev)
        {
            while (startRev < endRev)
            {
                char temp = str[startRev];
                str[startRev] = str[endRev];
                str[endRev] = temp;
                startRev++;
                endRev--;
            }
        }

        sentence = System.Text.RegularExpressions.Regex.Replace(sentence, "\\s+", " ").Trim();

        char[] charArray = sentence.ToCharArray();

        StrRev(ref charArray, 0, charArray.Length - 1);

        for (int start = 0, end = 0; end <= charArray.Length; ++end)
        {
            if (end == charArray.Length || charArray[end] == ' ')
            {
                StrRev(ref charArray, start, end - 1);
                start = end + 1;
            }
        }

        return new string(charArray);
    }
}
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
