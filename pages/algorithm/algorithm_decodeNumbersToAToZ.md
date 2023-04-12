
### Example




## Solution 2: Recursion，很慢！几乎是倒数 1%，不要用这个方法
这题用 Recursive way 慢的原因是：比如数组中间的一个两位数，每次别人经过它的时候都要判断一次它是否valid，如果它左边一共有x位，一共有了y种valid way，那么在它这里就要搞y次验证

### Complexity
* Time: O(2^n)  <=== 对么？
* Space: O(n)，call stack应该是n层

### Java
```java
class Solution {
    int numOfWays = 0;
    
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        char[] cArray = s.toCharArray();
        dfs(cArray, 0);
        return numOfWays;
    }
    
    private void dfs(char[] cArray, int curIndex) {
        if (curIndex == cArray.length) {
            numOfWays++;
            return;
        }
        
        if (validSingleDigit(cArray, curIndex)) {
            dfs(cArray, curIndex + 1);
        }
        if (validDoubleDigits(cArray, curIndex)) {
            dfs(cArray, curIndex + 2);
        }
    }
    
    private boolean validSingleDigit(char[] cArray, int curIndex) {
        char curChar = cArray[curIndex];
        
        // 'A' was marked as 1 in this question, not 0,
        // so only 9 chars starting with 'A' are represented by one digit number
        return (curChar - '0' >= 1) && (curChar - '0' <= 9);
    }
    
    private boolean validDoubleDigits(char[] cArray, int curIndex) {
        if (curIndex > cArray.length - 2) {
            return false;
        }
        
        // 两位里的十位不能是0！是0就是invalid的两位数
        if (cArray[curIndex] == '0') {
            return false;
        }
        
        int number = 0;
        number += (cArray[curIndex + 1] - '0');
        number += (cArray[curIndex] - '0') * 10;
        return (number >= 10) && (number <= 26);
    }
}
```

## Reference
* [Decode Ways [LeetCode]](https://leetcode.com/problems/decode-ways/description/)

{% include links.html %}
