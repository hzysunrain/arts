##  1. Algorithm 

### (1).整数反转

给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

例如：
```
输入: 123
输出: 321
```

代码：
```
class Solution {
     public int reverse(int x) {
        int ret = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (ret > Integer.MAX_VALUE / 10 || (ret == Integer.MAX_VALUE / 10 && pop > 7)) {
                return 0; //正数溢出
            }
            if (ret < Integer.MIN_VALUE / 10 || (ret == Integer.MIN_VALUE / 10 && pop < -8)) {
                return 0; //负数溢出
            }
            ret = ret * 10 + pop;
        }
        return ret;
    }
}
```
### (2). 回文数

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

例如：
```
输入: 121
输出: true
```
代码：
```
class Solution {
    public boolean isPalindrome(int x) {
        if (x == 0) {
            return true;
        }
        if (x < 0 || x % 10 == 0) {
            return false;
        }
        int rev = 0;
        while (x >rev) {
            rev = rev * 10 + x % 10;
            x /= 10;
        }
        return x == rev || x == rev/10;
    }
}
```

## 2. Review
#### [curl exercises]
   curl 是用于发起http请求的工具，能够很好地测试服务和api接口。
   [原文链接]https://jvns.ca/blog/2019/08/27/curl-exercises/

## 3. Tip

#### [Mysql 服务器性能剖析]

#### （1）.度量
  数据库服务器的性能用查询的响应时间来度量，单位是每个查询花费的时间。
  
#### （2）.通过性能剖析进行优化
  性能剖析是测量和分析时间花费在哪里的主要方法。
  
  性能剖析一般有连个步骤： 
  + 测量任务所花费的时间
  + 对结果进行统计和排序，将重要的任务排到前面。
  
  性能剖析类型：
  + 基于执行时间的分析（研究的是什么任务的执行时间最长）
  + 基于等待的分析（判断任务在什么地方被阻塞的时间最长）
  
#### （3） 剖析方法和工具

  + show status
  
  + show profile
  
  + 检查慢查询日志
  
  + explain 分析语句

  + pt-query-digest
  
  + performance schema
  
  + 使用 USER_STATISTICS 表
  
    ```
    SHOW TABLES FROM INFORMATION_SCHEMA LIKE '%_STATISTICS'
    ```
   + 使用 strace （但不适合在生产环境中使用）
   
    ```
    strace -cfp $(pidof mysqld)
    ```
    
    + pt-ioprofile （生成I/O活动的剖析报告）
    
## 4. Share

### 【ETCD 与 服务发现】
   ETCD用于配置共享和服务发现的键值存储系统，其四个核心特点：
   
   简单：基于HTTP+JSON的API让你用curl命令就可以轻松使用。
   
   安全：可选SSL客户认证机制。
   
   快速：每个实例每秒支持一千次写操作。
   
   可信：使用Raft算法充分实现了分布式。
  [文章链接]https://www.jianshu.com/p/3bd041807974


    


