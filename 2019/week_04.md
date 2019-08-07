##  1. Algorithm 

### 最长回文子串

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

示例 ：
```
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
```

代码：
```
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 1) {
            return "";
        } else if (s.length() == 1) {
            return s;
        }
        int start = 0, end = 0;
        int len1 , len2 , len;
        for (int i = 0; i < s.length(); i++) {
            len1 = expandAroundCenter(s, i, i);
            len2 = expandAroundCenter(s, i, i + 1);
            len = Math.max(len1, len2);
            if (len > end - start) {
                start = i - (len - 1) / 2;
                end = i + len / 2;
            }
        }
        return s.substring(start, end + 1);
    }
    
    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1;
    }
}
```

## 2. Review
#### [What does debugging a program look like]
   调试对于开发者来说，是一个很重要的能力。优秀的调试能力，能快速帮助你找到bug，提高你的工作效率；而写出有利于调试的代码，
   也是一个优秀程序员所必备的能力。
   + 重现bug
   + 快速重现bug
   + 接受你的代码有错误
   + 开始尝试修改
   + 一次只修改一个
   + 检查你的假设
   + 用非常规的或者奇异的方法去获取信息
   + 编写容易调试的代码
   + 输出错误信息比直接失败好
   + 打印更多的错误，而不仅仅是一个错误
   + 了解错误信息的含义
   
   [原文链接]https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/

## 3. Tip

#### [ORM、 Sql Builder 和 Sql语句常量]

+ ORM (Object Relational Mapping) 对象关系映射
  
  就是把数据库映射为程序的对象或结构。这样可以屏蔽sql语法细节，提高编程效率。 
  但正因为屏蔽了太多的细节，反而对sql语句的控制力变弱，后期也更难维护。
  因此很多人认为应该去ORM化。但因其简单性，受到很多初学者的喜爱。
  
+ Sql Builder (sql 语句构造器)

  就是通过 sql 构造器来构造sql语句，这种方式相比ORM来说，隐藏的细节更少，写法也相对简单，
  很好地平衡了sql的编程性和维护性。 对于写运营后台这类对性能要求不高的应用是一个不错的选择。
  
+ Sql 语句常量 *

  把sql语句通过常量的方式，放在一个明显的位置。 这个方便DBA对sql语句进行审核，能够很好的控制sql语句的安全和性能，
  非常有利于维护。 因此很好兼顾了sql语句的性能、安全和维护性,是开发高性能和高可用性应用的优选方式。
  
  例如：
  ```
    const getUserById = "select * from user where id=?"
  
    ...
  ```
  
  
## 4. Share

### 【你的项目应该如何正确分层】

   阿里的分层规范：
   1. 最上层是 controller和TService 层
   2. Service：业务层
   3. Manager：可复用逻辑层
   4. DAO：数据库访问层

  [文章链接]https://juejin.im/post/5b44e62e6fb9a04fc030f216


    


