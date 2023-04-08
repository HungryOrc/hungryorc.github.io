

## Solution 2，先排序数组，然后两边向中间逼近
indexLeft, indexRight 分别指向数组第一个元素和最后一个元素，判断两个元素的和 targetSum 的大小关系
* 如果 A[indexLeft] + A[indexRight] == targetSum，那么找到两个下标返回即可
* 如果 A[indexLeft] + A[indexRight] < targetSum，说明两个数的和还不够大，把 indexLeft 右移
* 否则两个数和太大，把 indexRight 左移
* 直到两个 index 重合

### Complexity
* Time: O(n * logn)，其中排序用 O(n * logn)，两边向中间逼近用 O(n)
* Space: O(n)，size of map

