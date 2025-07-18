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
### Largest Unique Substring 
```
    private static bool DoesStringExist(string s, List<string> currentCharacters)
    {
        foreach (var currentCharacter in currentCharacters)
        {
            if (s == currentCharacter) return true;
        }

        return false;
    }

    public static int LengthOfLongestSubstring(string s)
    {
        // use 2 pointers method
        int start = 0;
        int current = start;
        int end = s.Length - 1;
        int currentLength = 0;
        int maxLength = 0;
        List<string> currentCharacters = new List<string>();

        string startVal = null;
        string currentVal = null;
        string endVal = null;

        while (current <= end)
        {
            startVal = s[start].ToString();
            currentVal = s[current].ToString();
            endVal = s[end].ToString();

            if (current == start)
            {
                currentCharacters.Add(startVal);
                current++;
                continue;
            }

            if (!DoesStringExist(currentVal, currentCharacters))
            {
                currentCharacters.Add(currentVal);
                current++;
            }
            else
            {
                currentLength = currentCharacters.Count;
                if (maxLength <= currentLength) maxLength = currentLength;
                currentCharacters.Clear();
                start++;
                current = start;
            }
        }

        currentLength = currentCharacters.Count;
        if (maxLength <= currentLength) maxLength = currentLength;
        return maxLength;
    }
```

## Fast and Slow Pointers

## Sliding Window
### Reverse a LinkedList
```
public ListNode ReverseList(ListNode head) 
    {
        ListNode high = head;
        ListNode mid = null;
        ListNode low = null;

        while(high != null)
        {
            low = mid;
            mid = high;
            high = high.next;
            mid.next = low;
        }

        high = mid;
        return high;
    }
```

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

### Introduction to Graphs
A graph is a nonlinear data structure that represents connections between entities. In graph theory, entities are represented as vertices (or nodes), and the connections between them are expressed through edges.

Undirected Graphs - 2-way relationship
Directed Graph - 1-way relationship
Weighted Graph - a cost or measure is associated with each edge
Cyclic Graph - a graph that contains at least 1 cycle. A -> B -> C -> A 
Acyclic Graph - a graph that contains no cycle.

For graph related coding questions, an adjacency list or matrix will be provided. **Breadth First Search(BFS) and Depth First Search(DFS) are graph traversal techniques**.

In **DFS**, the strategy is to explore  as far as possible along one path before turning back. Visit all nodes and neighbours marking them as visited. Once a node with no neighbours are found, the algorithms starts backtracking. In backtacking, the algorithm  goes one step back and checks for the remaining neighbour nodes that are yet to be explored, this continues until all nodes reacheable from the source node has been visited.

**The DFS algorithm can be implemented with a Stack**.  Initialize an empty stack and choose a source node, push it onto the stack, and mark it as visited. While the stack is not empty, pop a node, explore its unvisited neighbors, and push them onto the stack, marking them as visited. Continue this process until reaching a node with no unvisited neighbors, then backtrack by popping from the stack. Repeat until the stack is empty, ensuring all connected nodes are visited.

In **BFS**, the strategy is to explore the graph in layers, one level at a time. The algorithm begins at a chosen source node and visits all its immediate neighbor nodes while marking them as visited. It then moves on to visit the neighbors of those nodes before proceeding to the next level of neighbors. This process continues until all the nodes in the graph, that are reachable from the source node, have been visited.

The **BFS** algorithm can be implemented with a queue.  Initialize an empty queue and choose a source node, enqueue it, and then enter a loop. Within this loop, the algorithm dequeues a node from the front of the queue, visits its immediate neighbors, and marks them as visited. These neighbors are subsequently enqueued into the queue. The queue plays a crucial role in determining the order of exploration, ensuring that nodes at the current level are processed before progressing to the next. This iterative process continues until the queue is empty, signifying that all reachable nodes from the source have been visited.

Traversals can be used to find paths, cycles, or connected components within graphs. However, when aiming for specific objectives, such as determining the shortest path rather than any path, or identifying the minimum spanning tree instead of connected components, traversals can be adapted to address particular optimization problems associated with the graph.

There are various algorithms that help us solve these specific graph problems. Let’s go over a few:

**Dijkstra’s algorithm**: It’s a variation of DFS and finds the shortest path between two nodes in a weighted graph.

**Bellman-Ford algorithm**: It’s a variation of BFS and finds the shortest paths in a weighted graph, even when negative edge weights are present.

**Floyd-Warshall algorithm**: It’s a variation of BFS and finds the shortest paths between all pairs of nodes in a weighted graph.

**Topological sorting**: It’s similar to DFS and orders nodes in a directed acyclic graph (DAG) to satisfy dependencies.

**Prim’s algorithm**: It finds the minimum spanning tree of a connected, undirected graph.

**Kruskal’s algorithm**: It also finds the minimum spanning tree of a connected, undirected graph.

Any problem with relationships between elements: There is a network of interconnected objects with some relationship between them; that is, the data can be represented as a graph.

Many problems in the real world use the graphs pattern. Let’s look at some examples.

Routing in computer networks: The graph representation helps to visualize the computer network, where nodes represent devices such as computers or servers and edges signify connections. Then, graph algorithms such as Dijkstra’s can be used to find the shortest and optimal path between any computer and the server. This is to ensure that data travels quickly from our computer to a faraway server on the internet without getting stuck or slowed down.

Flight route optimization: Airlines use graph-based algorithms to optimize flight routes. The airport network can be represented as a graph, where nodes represent airports, and edges represent flights connecting them. The algorithms can be used to find the shortest route between two airports, reduce fuel consumption, and minimize flight time.

Epidemic spread modeling: Graph algorithms can be used to predict how a virus might spread in a community. Imagine people as nodes and their interactions as edges. Algorithms simulate how a virus could move through these connections to help plan prevention strategies.

Recommendation systems: Online streaming services such as Netflix suggest movies we might like based on what we’ve watched using graph algorithms. Imagine all the movies and viewers as nodes, with edges connecting similar movies or people who like the same things. Graph algorithms then analyze these connections, finding patterns. If people who liked the same movies as we did also liked other movies we haven’t seen, the algorithm suggests those new movies to us. It’s like having a friend who knows our taste in movies and recommends something they think we’ll love based on what we’ve enjoyed before.

### Network Delay Time
Try to solve the Network Delay Time problem.
```
namespace Grokking_Coding_Interview.Graphs
{
    internal static class NetworkDelayTime
    {
        internal static int FindMinimumNetworkDelayTime(int[][] times, int n, int k)
        {
            //TODO create adjacency dictionary having key as source and List<int[]> as destination and travel time
            Dictionary<int, List<int[]>> adjacency = new Dictionary<int, List<int[]>>();
            
            // Store source as key, and destination and time as values

            foreach (int[] time in times)
            {
                int source = time[0];
                int destination = time[1];
                int travelTime  = time[2];

                if(!adjacency.ContainsKey(source)) adjacency[source] = new List<int[]>();
                adjacency[source].Add(new int[] { destination, travelTime });

            }
            //Print the adjacency dictionary
            PrintAdjacency(adjacency);

            //TODO create a proiority queue to store the delay time and the node.
            //TODO create a visited set to keep track of nodes that have been visited
            PriorityQueue<int[]> pq = new PriorityQueue<int[]>(Comparer<int[]>.Create((a, b) => a[0].CompareTo(b[0])));
            pq.Offer(new int[] { 0, k });
            HashSet<int> visited = new HashSet<int>();

            int delays = 0;

            while (pq.Count() > 0)
            {
                int[] current = pq.Poll();
                int time = current[0];
                int node = current[1];

                Console.WriteLine("\t Retrieved time: " + time);
                Console.WriteLine("\t Retrieved node: " + node);

                if (visited.Contains(node)) continue;

                visited.Add(node);
                delays = Math.Max(delays, time);
                List<int[]> neighbors;
                if(!adjacency.TryGetValue(node, out neighbors)) neighbors = new List<int[]>();

                PrintNeighbors(neighbors);

                foreach (int[] neighbor in neighbors)
                {
                    int neighbourNode = neighbor[0];
                    int neighbourTime = neighbor[1];

                    if (!visited.Contains(neighbourNode))
                    {
                        int newTime = time + neighbourTime;
                        pq.Offer(new int[] { newTime, neighbourNode });
                    }
                }

            }

            if (visited.Count == n) return delays;
            return -1;
        }

        private static void PrintAdjacency(Dictionary<int, List<int[]>> adjacency)
        {
            Console.WriteLine("\t Adjacency dictionary: {");
            bool isFirst = true;

            foreach (var entry in adjacency)
            {
                if(!isFirst) Console.Write(", ");
                int source = entry.Key;
                List<int[]> destinations = entry.Value;
                Console.Write(source + ": [");

                for (int i = 0; i < destinations.Count; i++)
                {
                    int[] destination = destinations[i];
                    Console.Write("(" + destination[0] + ", " + destination[1] + ")");
                    if (i < destinations.Count - 1) Console.Write(", ");
                }

                Console.Write("]");
                isFirst = false;
            }
            Console.WriteLine("}");
        }

        private static void PrintNeighbors(List<int[]> neighbors)
        {
            Console.Write("\t Neighbors: [");
            for (int i = 0; i < neighbors.Count; i++)
            {
                int[] neighbor = neighbors[i];
                Console.Write("(" + neighbor[0] + ", " + neighbor[1] + ")");
                if (i < neighbors.Count - 1)
                {
                    Console.Write(", ");
                }
            }
            Console.WriteLine("]");
        }
    }
}
```

### Custom Priority Queue
```
namespace Grokking_Coding_Interview
{
    internal class PriorityQueue<T>
    {
        private List<T> heap;
        private IComparer<T> comparer;

        public PriorityQueue(IComparer<T> comparer)
        {
            heap = new List<T>();
            this.comparer = comparer;
        }

        /// <summary>
        /// Acts as a min heap - a tree that will always have 2 children 
        /// </summary>
        /// <param name="item"></param>
        public void Offer(T item)
        {
            heap.Add(item);
            int i = heap.Count - 1;

            while (i > 0)
            {
                int parent = (i - 1) / 2;
                if (comparer.Compare(heap[parent], heap[i]) <= 0) break;
                Swap(i, parent);
                i = parent;
            }
        }

        public T Poll()
        {
            if(heap.Count == 0) throw new InvalidOperationException("Priority Queue is empty");
            T result = heap[0];
            int last = heap.Count - 1;
            heap[0] = heap[last];
            heap.RemoveAt(last);

            int i = 0;

            while(true)
            {
                int leftChild = 2 * i + 1;
                if (leftChild >= heap.Count) break;

                int rightChild = 2 * i + 2;
                int minChild = (rightChild < heap.Count && comparer.Compare(heap[rightChild], heap[leftChild]) < 0) ? rightChild : leftChild;

                if (comparer.Compare(heap[i], heap[minChild]) <= 0) break;

                Swap(i, minChild);
                i = minChild;
            }

            return result;
        }

        public int Count()
        {
            return heap.Count;
        }

        private void Swap(int i, int j)
        {
            T temp = heap[i];
            heap[i] = heap[j];
            heap[j] = temp;
        }
    }
}

```

### Paths in Maze That Lead to Same Room
Try to solve the Paths in Maze That Lead to Same Room problem.

```
public static int NumberOfPaths(int n, int[][] corridors)
{
    //TODO Create an adjacency matrix to store the rooms as keys and its neighbor rooms as the corresponding values. Create a counter to store the number of cycles.
    Dictionary<int, HashSet<int>> adjacencyDictionary = new Dictionary<int, HashSet<int>>();
    int cycles = 0;
    //TODO Start iterating over corridors, and store the neighbors of room1 and room2 in the matrix.

    foreach (int[] corridor in corridors)
    {
        int room1 = corridor[0];
        int room2 = corridor[1];

        if (!adjacencyDictionary.ContainsKey(room1))
        {
            adjacencyDictionary.Add(room1, new HashSet<int>());
            adjacencyDictionary[room1].Add(room2);
        }

        else
        {
            adjacencyDictionary[room1].Add(room2);
        }

        if (!adjacencyDictionary.ContainsKey(room2))
        {
            adjacencyDictionary.Add(room2, new HashSet<int>());
            adjacencyDictionary[room2].Add(room1);
        }

        else
        {
            adjacencyDictionary[room2].Add(room1);
        }

        //TODO Take the intersection of the neighbors of room1 and room2 and add the length of the result to the counter. What does this mean?
        int intersections = FindIntersections(adjacencyDictionary[room1], adjacencyDictionary[room2]);
        cycles += intersections;
    }

    //TODO Repeat this process until we have iterated over all corridors.
    return cycles;
}

private static int FindIntersections(HashSet<int> firstSet, HashSet<int> secondSet)
{
    int intersections = 0;
    foreach (int number in firstSet)
    {
        if (secondSet.Contains(number)) intersections++;
    }

    return intersections;
}
```

### Clone Graph
Try to solve the Clone Graph problem.

```
public static Node CloneHelper(Node root, Dictionary<Node,Node> nodesCompleted)
  {
    if(root == null)
    {
      return null;
    }
    
    Node clonedNode = new Node(root.data);
    nodesCompleted[root] = clonedNode;
    
    foreach(Node p in root.neighbors)
    {
      Node x;
      
      if(nodesCompleted.TryGetValue(p, out x))
      {
        clonedNode.neighbors.Add(x);
      }
      else
      {
        clonedNode.neighbors.Add(CloneHelper(p, nodesCompleted));
      }
    }
    
    return clonedNode;
  }
  
  public static Node Clone(Node root) 
  {
    Dictionary<Node, Node> nodesCompleted = new Dictionary<Node, Node>();
    return CloneHelper(root, nodesCompleted);
  }
```

## Graph Valid Tree
Try to solve the Graph Valid Tree problem.

```
using System;
using System.Collections.Generic;

public class Solution 
{
    public static bool IsGraphAValidTree(int n, int[][] edges) 
    {
        
        // Replace this placeholder return statement with your code
        /*
            n = 4
            edges = [[0,1],[1,2],[0,2]
        */
        if(edges.Length != n-1) return false;
        

        //need adjacency list
        Dictionary<int, HashSet<int>> adjacencyList= new();
        //need a set to track nodes
        HashSet<int> visitedNodes = new();
        
        //fill the adjacency list
        foreach(int[] connection in edges)
        {
            int node1 = connection[0];
            int node2 = connection[1];
            
            if(!adjacencyList.ContainsKey(node1))
            {
                adjacencyList.Add(node1, new());
                adjacencyList[node1].Add(node2);
            }
            
            else
            {
                adjacencyList[node1].Add(node2);
            }
            
            if(!adjacencyList.ContainsKey(node2))
            {
                adjacencyList.Add(node2, new());
                adjacencyList[node2].Add(node1);
            }
            else
            {
                adjacencyList[node2].Add(node1);
            }
        }
        
        //Adjacency List looks like this
        /*
        0: 1
        1: 0, 2
        2: 1
        3: 
        */
        PrintAdjacencyList(adjacencyList);
            
        //need a stack
        Stack<int> stack = new();
        // TODO if all nodes are connected and
        // TODO if no cycle between them return true
        return false;
    }
    
    private static void PrintAdjacencyList(Dictionary<int, HashSet<int>> adjacencyList)
    {
        foreach(var kvp in adjacencyList)
        {
            var key = kvp.Key;
            var values = GetHashSetValues(kvp.Value);
            Console.WriteLine($"key :{key} contains {values}");
        }
    }
    
    private static string GetHashSetValues(HashSet<int> hashset)
    {
        string output = "";
        bool first = true;
        
        foreach(var value in hashset)
        {
             if(!first) output += ", ";
             first = false;
             output += $"{value}";
        }
        
        return output;
    }
}
```

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
