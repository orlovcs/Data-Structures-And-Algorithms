Create a function to determine the longest common prefix string within an array of strings.

If no common prefix exists, return an empty string "".

### Examples:

#### Example 1:
- **Input**: `strs = ["interstellar", "internet", "interval"]`
- **Output**: `"inte"`

#### Example 2:
- **Input**: `strs = ["apple", "banana", "cherry"]`
- **Output**: `""`
- **Explanation**: There is no common prefix among the input strings.

# Intuition
Compare the shortest and longest strings.

# Complexity
- Time complexity:
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity:
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

# Code
```
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        mins = min(strs)
        maxs = max(strs)
        k = ""
        for i in range(len(mins)):
            if (mins[i] == maxs[i]):
                k+=mins[i]
            else:
                break
        return k        
```

[Source](https://leetcode.com/problems/longest-common-prefix/description/?source=submission-ac)