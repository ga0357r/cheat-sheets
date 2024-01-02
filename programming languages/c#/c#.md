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
