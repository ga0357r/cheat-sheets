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
    from g in arr.GroupBy(x => x)
    where g.Count() == 1
    select g.First();

return q.FirstOrDefault();
```
