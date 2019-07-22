##  1. Algorithm 

### 无重复字符的最长子串

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:

```
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

示例 2:

```输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```
示例 3:

```
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```
代码：
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), maxLen = 0;
        int[] array = new int[128];
        
        for (int i = 0, j = 0; j < n; j++) {
            i = Math.max(array[s.charAt(j)], i);
            maxLen = Math.max(maxLen, j - i + 1);
            array[s.charAt(j)] = j + 1;
        }
        
        return maxLen;
    }

}
```

## 2. Review

### What Is New in MySQL 8.0 （mysql 8.0 添加的新功能）

#### 1.Data dictionary.
     数据字典
#### 2.Atomic data definition statements (Atomic DDL).
     原子数据定义语句
#### 3.Upgrade procedure.
     升级过程
#### 4.Security and account management
     安全和账号管理
#### 5.Resource management.
     资源管理
#### 6.Table encryption management.
     表加密管理
#### 7.InnoDB enhancements. 
     InnoDB 引擎增强
#### 8.Character set support. 
     字符集支持
#### 9.JSON enhancements. 
     json格式字段增强
#### 10.Data type support.  
     数据类型支持
#### 11.Optimizer.
     优化
#### 12.Common table expressions.
     通用表表达式
#### 13.Window functions. 
     窗口函数， 
#### 14.Lateral derived tables. 
     侧向派生表
#### 15.Aliases in single-table DELETE statements. 
     单表删除语句中的使用别名
#### 16.Regular expression support. 
     正则表达式支持
#### 17.Internal temporary tables. 
     内部临时表
#### 18.Logging
     日志
#### 19.Backup lock. 
     备用锁
#### 20.Replication
     主从复制
#### 21.Connection management. 
     连接管理
#### 22.Configuration.
     配置项
[more]https://dev.mysql.com/doc/refman/8.0/en/mysql-nutshell.html

## 3. Tip

#### [缓存架构]

    使用双淘汰法是一个不错的选择
    
    1. 先淘汰缓存
    2. 写数据库
    3. 1s后，再淘汰缓存(采用消息队列来异步淘汰缓存)
    
    参考：
    [链接]https://www.w3cschool.cn/architectroad/architectroad-cache-architecture-design.html

## 4. Share

### 【GMTC2019分享记录｜AR在百度小游戏中的探索与实践】

    [文章链接]https://baijiahao.baidu.com/s?id=1637560533467534453&wfr=spider&for=pc
    
    AR游戏，下一代游戏风口！！！
    


