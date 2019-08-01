##  1. Algorithm 

### 寻找两个有序数组的中位数

给定两个大小为 m 和 n 的有序数组 nums1 和 nums2。

请你找出这两个有序数组的中位数，并且要求算法的时间复杂度为 O(log(m + n))。

你可以假设 nums1 和 nums2 不会同时为空。

示例 1:
```
nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0
```
示例 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```
代码：
```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        if (m > n) {
            int[] tempArr = nums1;
            nums1 = nums2;
            nums2 = tempArr;
            int tmpInt = m;
            m = n;
            n = tmpInt;
        }
        int iMin = 0, iMax = m, middleLen = (m + n + 1) / 2;
        while (iMin <= iMax) {
            int i = (iMin + iMax) / 2;
            int j = middleLen - i;
            if (i < iMax && nums2[j - 1] > nums1[i]) {// i 小
                iMin = i + 1;
            } else if (i > iMin && nums1[i - 1] > nums2[j]) { // i 大
                iMax = i - 1;
            } else { // i 匹配
                int maxLeft;
                if (i == 0) {
                    maxLeft = nums2[j - 1];
                } else if (j == 0) {
                    maxLeft = nums1[i - 1];
                } else {
                    maxLeft = Math.max(nums1[i - 1], nums2[j - 1]);
                }
                if ((m + n) % 2 == 1) {
                    return maxLeft;
                }

                int minRight;
                if (i == m) {
                    minRight = nums2[j];
                } else if (j == n) {
                    minRight = nums1[i];
                } else {
                    minRight = Math.min(nums2[j], nums1[i]);
                }

                return (maxLeft + minRight) / 2.0;
            }
        }
        return 0.0;
    }
}
```

## 2. Review
#### [Get your work recognized: write a brag document]
   如何让别人认可你的工作，这个是一个很重要的问题。 因为只有体现你的价值，才能让别人认可你，你才能获得更多的收获。
   
   [文章链接]https://jvns.ca/blog/brag-documents/

## 3. Tip

#### [InnoDB 的多版本并发控制(MVCC)]

    InnoDB 的MVCC，是通过在每行记录后面保存两个隐藏的列来实现的。这两个列，一个保存了行的创建时间，一个保存了行的过期时间(或删除时间)。当然存储的并不是实际的时间值，而是系统版本号(system version number)。每开始一个新的事务，系统版本号就会自动递增。事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。
    以下是在 REPEATABLE READ 隔离级别下，MVCC具体流程：
    
    SELECT 操作：
        InnoDB 会根据以下两个条件来检查每行记录：
          1. InnoDB 只查询早于当前事务版本的数据行（也就是，行的系统版本号小于或等于事务的系统版本号）
           这样可以确保事务读取的行，要么是在事务开始前已经存在的，要么就是事务自身插入或修改过的。
          2. 行的删除版本要么未定义，要么大于当前事务版本号。这样可以确保事务读取到的行，在事务开始之前未被删除。
        只有符合上述两个条件的记录，才能返回作为查询结果。
        
    INSERT 操作：
        InnoDB 为新插入的每一行保存当前系统版本号作为行版本号。
        
    DELETE 操作：
        InnoDB 为删除的每一行保存当前系统版本号作为行删除标识。
       
    UPDATE 操作：
        InnoDB 为插入一行新记录，保存当前系统版本号作为行版本号，同时保存当前系统版本号到原来的行作为行删除标识。
       
    MVCC 只在REPEATABLE READ 和 READ COMMITTED 这两个隔离级别下工作。

## 4. Share

### 【布隆过滤器】

本质上布隆过滤器是一种数据结构，比较巧妙的概率型数据结构（probabilistic data structure），特点是高效地插入和查询，可以用来告诉你 “某样东西一定不存在或者可能存在”。
相比于传统的 List、Set、Map 等数据结构，它更高效、占用空间更少，但是缺点是其返回的结果是概率性的，而不是确切的。

[文章链接]https://www.jianshu.com/p/2104d11ee0a2

    


