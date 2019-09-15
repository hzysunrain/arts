##  1. Algorithm 

### (1).字符串转换整数 (atoi)

实现一个 atoi 函数，使其能将字符串转换成整数。

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。
如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。


例如：
```
输入: "42"
输出: 42
```

代码：
```
class Solution {
     public int myAtoi(String str) {
        str = str.trim();
        if (str.length() == 0){
            return 0;
        }
        int sign = 1;
        int start = 0;
        long ret = 0;
        char first = str.charAt(start);
        if (first == '-'){
            sign = -1;
            start++;
        }else if(first == '+'){
            start++;
        }
        for(;start < str.length();start++){
            if (!Character.isDigit(str.charAt(start))){
                return (int)ret * sign;
            }
            ret = ret *10 +str.charAt(start) -'0';
            if (sign== 1 && ret > Integer.MAX_VALUE){
                return Integer.MAX_VALUE;
            }
            if (sign == -1 && ret > Integer.MAX_VALUE){
                return Integer.MIN_VALUE;
            }
        }

        return (int)ret*sign;
    }
}
```


## 2. Review
#### [git exercises: navigate a repository]
   使用真实的仓库来练习git的命令。
   ```
   git checkout
   git log (--oneline, --author, and -S will be useful)
   git diff (--stat will be useful)
   git show
   git status
```
   
   [原文链接]https://jvns.ca/blog/2019/08/30/git-exercises--navigate-a-repository/

## 3. Tip

#### [Mysql InnoDB 行锁]

#### （1）.两阶段锁协议
  在InnoDB事务中，行锁是在需要的时候才加上去的，但并不是不需要了就立刻释放，而是要等到事务结束时才释放。
  
  因此，在你的事务中需要锁定多个行时，要把最可能造成锁冲突、最可能影响并发度的锁尽量往后面放。
  
  调整锁顺序的方法只能降低死锁发送的几率，并不能消除死锁、
  
#### （2）.死锁和死锁检测
  死锁：当并发系统中不同的线程出现循环资源依赖，涉及的线程都在等到别的线程释放资源时，就会导致这几个线程进入无限等待的状态，这就是死锁。
  
  解决死锁有两种方法： 一是设置死锁超时时间， 一是进行死锁检测。

  死锁超时时间： 线程直接进入等待，直至超时。 通过 innodb_lock_wait_timeout 来设置超时时间。一般不通过该方法来解决死锁，因为超时时间难以确定一个合理数值，同时效率较低。
    
  死锁检测： 当死锁检测发现死锁后，主动回滚死锁链条中的某一个事务，让其他事务可以继续执行。 这个是推荐的方法，但需要消耗更多的资源，不过可以通过控制客户端的并发度来解决这个问题。
  
  将参数 innodb_deadlock_detect 设置为on，表示开启这个逻辑。 mysql 默认是开启这个配置的、
  
  控制客户端并发数的方式：
     
     a. 直接修改mysql源码，对相同行的更新，在进入引擎前进行排队，
     b. 根据业务做详细设计，可以把一行的修改变成逻辑上的多行来减少锁的冲突。
         
## 4. Share

### 【程序员最重要的技能：知道什么时候不写代码】
   
   本文指出大多数程序员都容易犯下的错是，因为对编程的兴奋，不知道什么时候应该对编码说“不”。程序员需要知道什么时候不需要编码，并从项目中删除所有不必要的代码，这将让工作变得更容易，并使软件寿命更持久。
   
  [文章链接] https://mp.weixin.qq.com/s?__biz=MjM5MDE0Mjc4MA==&mid=2651019484&idx=1&sn=034e678002cee61027c63f20887011f2&chksm=bdbea08f8ac9299943b8f45c46bfd3e87580c75928689635f1b97619bf1d8defb0ef8b8e0db6&mpshare=1&scene=1&srcid=&sharer_sharetime=1568523302846&sharer_shareid=577f7b5c8b4142aa1752a25e3d8df7b2#rd


    


