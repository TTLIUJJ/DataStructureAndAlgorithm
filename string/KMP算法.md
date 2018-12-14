# KMP算法

```
前缀和后缀的定义
eg. 
m: abc1234abcxyz
定位x元素，它的前缀和后缀相等的最大长度为3, 并且值为"abc"
并且, 前缀不能包含后缀的最后一个, 一定要包含字符串第一个
同样, 后缀不能包含前缀的第一个, 一定要包含第i-1个(字符串以i结尾的后缀)
eg.
	aaaab
b的前缀最长为[0, 2]
b的后缀最长为[1, 3]

注: 人为规定, m第一个元素的next值为-1, 第二个元素的next值为0


next数组保存的信息
eg.
m   : "abc1234abcxyz"
m   : [ a, b, c, 1, 2, 3, 4, a, b, c, x, y, z]
next: [-1, 0, 0, 0, 0, 0, 0, 0, 1, 2, 3, 0, 0]

next数组保存的信息分别两点:
1. 前缀和后缀相等的最大长度
2. 前缀的下一个需要比较的下标

分析kmp算法流程
eg.
next数组中，y位置保存的信息应该是5, 表示y的前缀和后缀相等的最大长度为5
n : 1234abcdef123abcdefx
m :     abcdef123abcdefy
===> 此时n配不上m字符串, 那么m往右推多个位置，如下，
     而不是仅仅从往右推1格，
n : 1234abcdef123abcdefx
m :              abcdef123abcdefy 

kmp算法的优化点: 前缀和后缀相等，那么m往右移动大于等于1个位置，并且还是从x的位置开始新一轮的匹配

构造next数组
eg. 求此时z的next值
m   : a b a c d a b a t k s a b a c d a b a y z
z的next值的查找过程如下,
y的相等缀长8, 第一次比较y和t, 结果不相等
t的相等缀长3, 第二次比较y和c, 结果不相等
c的相等缀长1, 第三次比较y和b, 结果不相等
b的相等缀长0, 第四次比较y和a, 结果不相等
next: a ||| b a || c d a b a | t k s a b a c d a b a | y z
```


```java
public class KMP {
    public static int getIndexOf(char []chars1, char []chars2) {
        if (chars1 == null || chars2 == null ||
            chars1.length < chars2.length ||
            chars2.length == 0) {

            return -1;
        }

        int []next = getNextArray(chars2);
        int x = 0;
        int y = 0;

        while (x < chars1.length && y < chars2.length) {
            if (chars1[x] == chars2[y]) {
                ++x;
                ++y;
            }
            else if (next[y] == -1) {
                ++x;
            }
            else {
                y = next[y];
            }
        }

        return y == chars2.length ? x - y : -1;
    }

    public static int []getNextArray(char []chars) {
        if (chars.length == 1) {
            return new int[] { -1 };
        }

        int []next = new int[chars.length];
        next[0] = -1;
        next[1] = 0;
        int cmp = 0;
        int   i = 2;

        while (i < next.length) {
            if (chars[i-1] == chars[cmp]) {
                next[i++] = ++cmp;
            }
            else if (cmp > 0) {
                cmp = next[cmp];
            }
            else {
                next[i++] = 0;
            }
        }

        return next;
    }

    public static void main(String []args) {
        String s1 = "abcabcababaccc";
        String s2 = "ababa";
        System.out.println(getIndexOf(s1.toCharArray(), s2.toCharArray()));
    }
}
```