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

```
public static string ReverseWords2(string sentence)
{
    static void ReverseString(char[] arr, int start, int end)
    {
        while(start < end)
        {
            Swap(arr, start, end);
            start++;
            end--;
        }

        static void Swap(in char[] arr, in int start, in int end, in bool preferTupleSwapping = true)
        {
            if (preferTupleSwapping)
            {
                (arr[start], arr[end]) = (arr[end], arr[start]);
                return;
            }

            else
            {
                var temp = arr[start];
                arr[start] = arr[end];
                arr[end] = temp;
            }
        }
    }

    //remove whitespace
    sentence = System.Text.RegularExpressions.Regex.Replace(sentence, "\\s+", " ").Trim();
    char[] charArr = sentence.ToCharArray();
    ReverseString(charArr, 0, charArr.Length - 1);
    int start = 0 , end = 0;

    while(end < charArr.Length)
    {
        if (charArr[end].ToString() == " ")
        {
            ReverseString(charArr, start, end - 1);
            start = end + 1;
            end = start;
            continue;
        }

        if(end == charArr.Length - 1)
        {
            ReverseString(charArr, start, end);
        }

        end++;
    }

    return new string(charArr);
}
```

### Valid Word Abbreviation
Try to solve the Valid Word Abbreviation problem.
```
public static bool ValidWordAbbreviation1(string word, string abbr)
{
    char[] wordArr = word.ToCharArray();
    char[] abbrArr = abbr.ToCharArray();
    int wordIndex = 0;
    int abbrIndex = 0;

    while (abbrIndex < abbrArr.Length)
    {
        if (char.IsDigit(abbrArr[abbrIndex]))
        {
            if (abbr[abbrIndex] == '0') return false;
            int number = 0;

            while (abbrIndex < abbrArr.Length && char.IsDigit(abbrArr[abbrIndex]))
            {
                number = number * 10 + (abbrArr[abbrIndex] - '0');
                abbrIndex++;
            }

            wordIndex += number;
        }

        else
        {
            if (wordIndex >= wordArr.Length || wordArr[wordIndex] != abbrArr[abbrIndex]) return false;
            wordIndex++;
            abbrIndex++;
        }
    }

    //have you reached the end of your string
    return wordIndex == wordArr.Length && abbrIndex == abbr.Length;
}

```

### Strobogrammatic Number
Try to solve the Strobogrammatic Number problem.

```
using System;
using System.Collections.Generic;
public class Solution
{
    public static bool IsStrobogrammatic (string num) 
    {
       //69
       // TODO Use a dictionary to map each digit to its valid rotated counterpart.
       Dictionary<string, string> StrobogrammaticMap = new Dictionary<string, string>
       {
          //6 -> 9 or they are the same number
          { "6", "9" },
          { "9", "6" }
        };

       // TODO Initialize two pointers: one at the start and one at the end of the string.
       // TODO Compare digits from both ends, to check each matches its valid rotation.
       // TODO Move both the pointers inwards until they cross.
       // TODO If any pair of digits does not match its valid rotation, return FALSE.
       // TODO Return TRUE if all pairs are valid according to the strobogrammatic rules.
       int start = 0, end = num.Length - 1;

       while (start < end)
       {
          string startString = num[start].ToString();
          string endString = num[end].ToString();
          string stroboValue = "";
          if(StrobogrammaticMap.TryGetValue(startString, out stroboValue)){}
          else{stroboValue = "";}
          bool isStrobogrammatic = stroboValue == endString;
          if (startString != endString && !isStrobogrammatic) return false;
          start++;
          end--;
       }
       
        return true;
    }
}
```

### Minimum Number of Moves to Make Palindrome
Try to solve the Minimum Number of Moves to Make Palindrome problem.
```
//wrong 2/5 in test cases. Need to improve
public class Solution {
    public int MinMovesToMakePalindrome(string s) {
        
        // ntiin
        // ccxx
        int moves = 0;
        int start = 0;
        int end = s.Length - 1;
        char[] stringArr = s.ToArray();

        while(start < end)
        {
          do
          {
            if(stringArr[end] == stringArr[end - 1])
            {
              var temp = stringArr[start + 1];
              stringArr[start + 1] = stringArr[start];
              stringArr[start] = temp;
            }
            else
            {
              moves++;
            }
            start++;
            end--;
          }
          while(start < end);
        }
        return moves;
    }
}
```

### Find the Container with The Most Water
```
public class Solution {

    private int GetMinNumber(int a, int b)
    {
        if(a < b)
        {
            return a;
        }
        else if(b < a)
        {
            return b;
        }
        else
        {
            return a;
        }
    }

    public int MaxArea(int[] height) {
        // 2 pointers low, high
        // move the pointer that points to the smaller height
        // if they are both the same numbers move them both forward
        int low = 0;
        int high = height.Length-1;
        int length = 0;
        int breadth = 0;
        int maxArea = 0;
        int currentArea = 0;

        while(low < high)
        {
            breadth = GetMinNumber(height[low], height[high]);
            length = high - low;
            currentArea = length*breadth;
            
            if(currentArea > maxArea)
            {
                maxArea = currentArea;
            }

            if(height[low] < height[high])
            {
                low++;
            }

            else if(height[high] < height[low])
            {
                high--;
            }

            else
            {
                low++;
                high--;
            }
        }

        return maxArea;
    }
}
```
### Word ladder 
```
public class Solution {
    public bool DoesWordExist(string word, IList<string> wordList)
    {
        int start = 0
        int end = wordList.Count;

        while(start <= end)
        {
            if(wordList[start] == word) return true;
            else if (wordList[end] == word) return true;

            start++;
            end--;
        }

        return false;
    }

    public int FindWordIndex(string word, IList<string> wordList)
    {
        int start = 0
        int end = wordList.Count;

        while(start <= end)
        {
            if(wordList[start] == word) return start;
            else if (wordList[end] == word) return end;

            start++;
            end--;
        }

        return -1;
    }

    public int LadderLength(string beginWord, string endWord, IList<string> wordList) {
        //does w2 exist in dictionary
        //loop through the dictionary
        // the numebr of times I move that will be the ladderLength
        
        if(DoesWordExist(endWord, wordList))
        {
            int pointer = 0;
            int wordListLength = wordList.Count;
            string currentWord = beginWord;
            int? number = null;

            //find beginWord
            int index = FindWordIndex(currentWord, wordList);

            if(index == -1)
            {
                number = 1
            }

            while(pointer < wordListLength && currentWord != endWord)
            {
                currentWord = wordList[pointer];
                mumber++;
                pointer++;
            }

            return number
        }

        else
        {
            return 0;
        }
        
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
