##  1. Algorithm 

### 正则表达式匹配

给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。


    '.' 匹配任意单个字符
    '*' 匹配零个或多个前面的那一个元素
    所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。

说明:

    s 可能为空，且只包含从 a-z 的小写字母。
    p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。

示例：

    输入:
    s = "aa"
    p = "a"
    输出: false
    解释: "a" 无法匹配 "aa" 整个字符串。

代码：
```
class Solution {
    public boolean isMatch(String s, String p) {
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[s.length()][p.length()] = true;

        for (int i = s.length(); i >= 0; i--) {
            for (int j = p.length() - 1; j >= 0; j--) {
                boolean firstMatch = (i < s.length() && (p.charAt(j) == s.charAt(i) || p.charAt(j) == '.'));
                if (j + 1 < p.length() && p.charAt(j + 1) == '*') {
                    dp[i][j] = dp[i][j + 2] || firstMatch && dp[i + 1][j];
                } else {
                    dp[i][j] = firstMatch && dp[i + 1][j + 1];
                }
            }
        }
        return dp[0][0];
    }
}
```

## 2. Review
#### [Where Is My Cache? Architectural Patterns for Caching Microservices]
   I’m sure you use caching somewhere in your system. This can be either to improve performance, reduce backend load, or to decrease downtime. Everybody uses caching. Caching is everywhere. However, in which part of your system should it be placed? If you look at the following diagram representing a simple microservice architecture, where would you draw the “cache” rectangle?
   
   [原文链接]https://dzone.com/articles/where-is-my-cache-architectural-patterns-for-cachi
   
## 3. Tip

#### [Mysql redo log、undo log、binlog]

##### (1). redo log （重做日志）

redo log 通常是物理日志，记录的是数据页的物理修改，而不是某一行或某几行修改成怎样怎样，它用来恢复提交后的物理数据页(恢复数据页，且只能恢复到最后一次提交的位置)。
     
##### (2). undo log （回滚日志）
undo log 用来回滚行记录到某个版本。undo log一般是逻辑日志，根据每行记录进行记录。

##### (3). binlog （归档日志）
binlog是属于MySQL Server层面的，又称为归档日志，属于逻辑日志，是以二进制的形式记录的是这个语句的原始逻辑，依靠binlog是没有crash-safe能力的

         
## 4. Share

### 【Golang并发：再也不愁选channel还是选锁】
   
   面对并发问题，是用channel解决，还是用Mutex解决？
   
   如果自己心里还没有清晰的答案，那就读下这篇文章，你会了解到：
   
   * 使用channel解决并发问题的核心思路和示例
   * channel擅长解决什么样的并发问题，Mutex擅长解决什么样的并发问题
   * 一个并发问题该怎么入手解解决
   * 一个重要的plus思维
   
  [文章链接] https://segmentfault.com/a/1190000017890174

    


