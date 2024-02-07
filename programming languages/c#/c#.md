# For Each Loop

```
foreach(var element in array){}
```

# Limit a float/double to 6 decimal points

```
float a = 0.5f;
a.ToString("N6");
//a is 0.5000000
```

# String Concatenation With Variables

```
int minSum = 15;
int maxSum = 20;
Console.WriteLine($"{minSum} {maxSum}");
//15 20
```

# Sort a List in Ascending Order

```
List<int> numbers = new List<int>{ 4, 2, 1, 5, 3 };
numbers.Sort();  // Sort the list in an ascending order
```

# Sort a List in Descending Order

```
List<int> numbers = new List<int>{ 4, 2, 1, 5, 3 };
numbers.Sort();  // Sort the list in an ascending order
numbers.Reverse(); // Reverse the order of the elements
```

# 64 bit integer

```
ulong unsignedLong = 40; //+ve number 0 - 18446744073709551615;
long signedLong = -40;  // +ve or -ve number -9,223,372,036,854,775,808 - 9,223,372,036,854,775,807
```

# Split a string based on delimiter
```
string s = "07:05:45PM";
var splitString = s.Split(':'); // [0] = "07", [1] = "05", [2] = "45PM"
```

# Remove a character from a
```
string s = "07:05:45PM";
var splitString = s.Split(':'); // [0] = "07", [1] = "05", [2] = "45PM"
string seconds = splitString[2].Remove(2); // Start Removing all elements from element 2. Remove "PM". Seconds = 45
```

# return a unique array element that appears once
```
int[] arr = { 2, 3, 4, 5, 8, 2, 3, 5, 4, 2, 3, 4, 6 };
var q =
    from g in arr.GroupBy(x => x)   //  Groups the array elements by their values, resulting in the following groups: {2, 2, 2}, {3, 3, 3}, {4, 4, 4}, {5, 5}, {8}, {6}.
    where g.Count() == 1 //Filters out groups with more than one element, leaving only {8}, {6}.
    select g.First(); //Selects the first (and only) element from each group, resulting in the sequence {8, 6}.

return q.FirstOrDefault(); //Returns the first element from the sequence, which is 8.
```

# Returning an absolute value and For Loops with tweaked variables
```
// For Loop
int column = arr.Count() -1;
int output = 0;

for(; column < arr[row].Count;)
{
    //get right-to-left diagonal
    output += arr[row][column];
    break;
}

//Absolute
int result = -20
int absoluteResult = Math.Abs(result); //20
```


# For Loops Default
```
for(int i = 0; i < 10; i++){}
```

# Create a List of 100 elements called 0
```
List<int> result = Enumerable.Repeat(0, 100).ToList();
```

# Make Sure a callback runs on Unity Main Thread
```
// Initialize the Google Mobile Ads SDK.
        MobileAds.Initialize(initStatus =>
        {
            UniRx.MainThreadDispatcher.Post(_ =>
            {
                UnityAds.SetConsentMetaData("gdpr.consent", true);
                ValidateMediationStatus(initStatus);
                Print.GreenLog("Google Mobile Ads Initialization Success");
                AdManager.Instance.LoadAd();
            }, null);
        });
```

# To Read data from the console
```
// read data from a line in console
var lineData = Console.ReadLine(); 
```

# To convert a string of numbers separated by space to a List
```
//Make sure to use System.Linq; using System.Linq;
// read data from a line in console
string array = Console.ReadLine(); //1 2 3 4 5 6 7
List<int> arrList = array.Split(' ').Select(int.Parse).ToList(); 
```

# To check the modulus
```
int m = 12
int n = 2
if(m % n == 0) return true;
```

# To keep an index within the bounds of a list size
```
public static int flippingMatrix(List<List<int>> matrix)
    {
        int matrixCount = matrix.Count();
        int sum = 0;
        List<int> elements = new List<int>();
        
        for(int i = 0; i < matrixCount / 2; i++)
        {
            for(int j = 0; j < matrixCount / 2; j++)
            {
                elements.Clear();
                
                elements = new List<int> 
                {
                    matrix[i][j],
                    matrix[i][matrixCount - j - 1],
                    matrix[matrixCount - i - 1][j],
                    matrix[matrixCount - i - 1][matrixCount - j - 1]
                };
                
                sum += elements.Max();
            }    
        }
        
        return sum;
    }
```

# To Replace a character in a string 
```
private static int ChooseIndexToRemove(int start, int end, string s)
{
    var charToRemove = s[start];
    s = s.Replace(charToRemove.ToString(), string.Empty);
    return start;
}
```

# To Convert a string array to a string datatype 
```
private static int ChooseIndexToRemove(int start, int end, string s)
{
    var charToRemove = s[start];
    s = s.Replace(charToRemove.ToString(), string.Empty);
    string reversedString = new string(s.Reverse().ToArray()); //Reversing a string is an expensive process. Beware
    return start;
}
```
