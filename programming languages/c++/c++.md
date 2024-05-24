# String and Vector and Array Size

```
// String #include <string>
string s = "test";
s.length(); // 4 

// Vector #include <vector>
vector<int> nums;
nums.size(); //0

//Array
int nums[4] = {1, 2, 3, 4};
nums.length(); // 4 
```

# Sorting an Array

```
#include <algorithm>
void SortList(std::vector<int> nums)
{
    std::sort(nums.begin(), nums.end());
}
```

# Splitting a string
```

#include <sstream>
#include <vector>
#include <string>

using namespace std;

vector<string> GetSplitString(const string& sentence, char delimiter = ' ')
{
  vector<string> words;
  stringstream ss(sentence);
  // ss.str()
  string word;

  // Split the string by the delimiter (default: space)
  while (getline(ss, word, delimiter)) {
    words.push_back(word);
  }

  return words;
}
```

# For each loop
```
#include <vector>
#include <iostream>

using namespace std;

vector<int> elements;

for(auto element : elements)
{

}
```

# using regular expression to remove whitespaces
```
#include <string>
#include <regex>

using namespace std;

string  sentence = "Hello   World   ";
sentence = std::regex_replace(sentence, std::regex("^ +| +$|( ) +"), "$1");
cout << sentence << endl;
//sentence = Hello World
```
