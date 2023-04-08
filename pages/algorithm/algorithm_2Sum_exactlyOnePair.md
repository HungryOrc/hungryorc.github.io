

## Solution 2，先排序数组，然后两边向中间逼近
indexLeft, indexRight 分别指向数组第一个元素和最后一个元素，判断两个元素的和 targetSum 的大小关系
* 如果 A[indexLeft] + A[indexRight] == targetSum，那么找到两个下标返回即可
* 如果 A[indexLeft] + A[indexRight] < targetSum，说明两个数的和还不够大，把 indexLeft 右移
* 否则两个数和太大，把 indexRight 左移
* 直到两个 index 重合

### Complexity
* Time: O(n * logn)，其中排序用 O(n * logn)，两边向中间逼近用 O(n)
* Space: O(n)，size of map

### Java
```java
public class Solution {
    public int[] twoSum(int[] givenNumbers, int targetSum) {
        int[] output = new int[2];

        int indexLeft = 0;
        int indexRight = givenNumbers.length - 1;

        // copy the given array
        int[] givenNumbersCopy = new int[givenNumbers.length];
        for (int i = 0; i < givenNumbers.length; i++) {
            givenNumbers_Copy[i] = givenNumbers[i];
        }

        // "Dual-Pivot" Quick Sort
        // Sort the copy of the given array, from smaller to bigger
        Arrays.sort(givenNumbersCopy);

        while (indexLeft < indexRight) {
            if (givenNumbersCopy[indexLeft] + givenNumbersCopy[indexRight] == targetSum) {
                // find the indexes of these 2 numbers in the original given array
                for (int i = 0; i < givenNumbers.length; i ++) {
                    if (givenNumbers[i] == givenNumbersCopy[indexLeft])	{
                        output[0] = i;
                        break;
                    }
                }
                for (int i = givenNumbers.length - 1; i >= 0; i --) {
                    if (givenNumbers[i] == givenNumbersCopy[indexRight])	{
                        output[1] = i;
                        break;
                    }
                }
            } else if (givenNumbersCopy[indexLeft] + givenNumbersCopy[indexRight] < targetSum) {
                indexLeft++;
            } else
                indexRight--;
            }
        }
        return output;
    }
}
```

