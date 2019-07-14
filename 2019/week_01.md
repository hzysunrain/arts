##  1. Algorithm 

### (1). 两数之和

给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。

示例:

```
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

代码:

```java
import java.util.HashMap;
import java.util.Map;

import static java.util.Arrays.sort;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        int end = nums.length - 1;
        if (end < 1)
            return new int[]{};

        int[] orgNums = nums.clone();
        sort(nums);
        int start = 0;
        while (start != end) {
            int sum = nums[start] + nums[end];
            if (sum == target) {
                int[] ret = new int[2];
                int index = 0;
                for (int i = 0; i < nums.length; i++) {
                    if (nums[start] == orgNums[i] || nums[end] == orgNums[i]) {
                        ret[index] = i;
                        if(index++ == 1){
                            return ret;
                        }
                    }
                }
            }
            if (sum > target) {
                end--;
            } else {
                start++;
            }
        }

        return new int[]{};
    }
}

```

### (2).两数相加

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

示例:

```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

代码:

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode sum = l1;
        ListNode last = l1;
        int carry = 0;
        while (l1 != null && l2 != null) {
            l1.val = l1.val + l2.val + carry;
            if (l1.val > 9) {
                carry = 1;
                l1.val = l1.val - 10;
            } else {
                carry = 0;
            }

            last = l1;
            l1 = l1.next;
            l2 = l2.next;
        }
        ListNode next = l1 == null ? l2 : l1;

        while (next != null) {
            next.val += carry;
            if (next.val > 9) {
                carry = 1;
                next.val = next.val - 10;
            } else {
                carry = 0;
            }
            last.next = next;

            last = last.next;
            next = next.next;
        }

        if (carry > 0) {
            last.next = new ListNode(carry);
        }
        return sum;
    }
}
```

## 2. Review

### 【Effective Dart: Style】

A surprisingly important part of good code is good style. Consistent naming, ordering, and formatting helps code that *is* the same *look* the same. It takes advantage of the powerful pattern-matching hardware most of us have in our ocular systems. If we use a consistent style across the entire Dart ecosystem, it makes it easier for all of us to learn from and contribute to each others’ code.

该文章主要讲解dart 语言的推荐代码风格。

文档链接 [https://dart.dev/guides/language/effective-dart/style] 

## 3. Tip

### 【类snowflake算法】

snowflake是twitter开源的分布式ID生成算法，其**核心思想**是：一个long型的ID，使用其中41bit作为毫秒数，10bit作为机器编号，12bit作为毫秒内序列号。这个算法单机每秒内理论上最多可以生成1000*(2^12)，也就是400W的ID，完全能满足业务的需求。

借鉴snowflake的思想，结合各公司的业务逻辑和并发量，可以实现自己的分布式ID生成算法。

举例，假设某公司ID生成器服务的需求如下：

（1）单机高峰并发量小于1W，预计未来5年单机高峰并发量小于10W

（2）有2个机房，预计未来5年机房数量小于4个

（3）每个机房机器数小于100台

（4）目前有5个业务线有ID生成需求，预计未来业务线数量小于10个

（5）…

分析过程如下：

（1）高位取从2016年1月1日到现在的毫秒数（假设系统ID生成器服务在这个时间之后上线），假设系统至少运行10年，那至少需要10年*365天*24小时*3600秒*1000毫秒=320*10^9，差不多预留39bit给毫秒数

（2）每秒的单机高峰并发量小于10W，即平均每毫秒的单机高峰并发量小于100，差不多预留7bit给每毫秒内序列号

（3）5年内机房数小于4个，预留2bit给机房标识

（4）每个机房小于100台机器，预留7bit给每个机房内的服务器标识

（5）业务线小于10个，预留4bit给业务线标识

![64bit标识](https://7n.w3cschool.cn/attachments/image/20170428/1493371116896750.png)

这样设计的64bit标识，可以保证：

（1）每个业务线、每个机房、每个机器生成的ID都是不同的

（2）同一个机器，每个毫秒内生成的ID都是不同的

（3）同一个机器，同一个毫秒内，以序列号区区分保证生成的ID是不同的

（4）将毫秒数放在最高位，保证生成的ID是趋势递增的

**缺点：**

（1）由于“没有一个全局时钟”，每台服务器分配的ID是绝对递增的，但从全局看，生成的ID只是趋势递增的（有些服务器的时间早，有些服务器的时间晚）

**最后一个容易忽略的问题：**

生成的ID，例如message-id/ order-id/ tiezi-id，在数据量大时往往需要分库分表，这些ID经常作为取模分库分表的依据，为了分库分表后数据均匀，ID生成往往有“取模随机性”的需求，所以我们通常把每秒内的序列号放在ID的最末位，保证生成的ID是随机的。

又如果，我们在跨毫秒时，序列号总是归0，会使得序列号为0的ID比较多，导致生成的ID取模后不均匀。解决方法是，序列号不是每次都归0，而是归一个0到9的随机数，这个地方。

注: 该方案文档出自 “58沈剑”。

## 4. Share

### 【Flutter vs React Native】

文章链接 [https://www.jianshu.com/p/ee42c567734f]

主要讲解了flutter 和react native 的优缺点，不过我经过一段时间使用flutter， 觉得flutter虽然还不稳定和完善，但还是很有前景的，觉得可以入坑flutter。

