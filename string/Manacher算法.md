# Manacher算法

```
暴力法:
以每个字符串为中心，判断其左右两边的相等的最长字符串
往字符串中间添加特殊字符，可以满足字符串长度为奇数和偶数的情况
eg.
m:  1 2 2 1 3 3 1 2 2 1
n: #1#2#2#1#3#3#1#2#2#1#
maxLength: 在最中间的位置，找到最长的回文半径 21/2 = 10
时间复杂度: O(n^2)

Manacher算法:
1. 回文半径c: 包含字符x在内的,回文半径
eg.
a#1#2#x#2#1#b
x的回文半径c为6, #1#2#x 或者 x#2#1#

2. 回文半径数组: 保存字符串m中每个字符的回文半径

3. 整体最右回文右边界: 
eg.
n   :    #  1  #  2  #  3  #  2  #  1   #
i   :    0  1  2  3  4  5  6  7  8  9  10
R   :-1  1  2  2  4  4 10 10 10 10 10  10

算法流程
- i处于R外面，使用暴力扩展
- i处于R里面，c一定位于i左边，那么肯定有i的对称点i'
	- i'的回文半径位于c的回文半径内
	- i'的回文半径超出c的回文半径，那么i的回文半径只有R-i
	- i'的回文半径，刚好与L重合·，那么需要验证以i为半径，验证R+1的位置

时间复杂度: O(n)

eg. 考虑i在R里面
n :     [          o          ]
                   c          R-1    回文半径:
p1:      [ i']           [ i ]     radius[2*c-i], i'的回文半径
p2:    [   i'  ]       [   i   ]       R-i, 去除不符合大于R边界的
p3:     [  i' ]         [  i  ]      需要验证
```

```java
public class Manacher {
    public static int maxLengthOfPalindrome(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        char []chars  = toManacherString(s);
        int  []radius = new int[chars.length];
        int R = -1; // 整体最右回文右边界的下一个位置   L[...C...]R
        int C = -1; // 整体最右回文时的回文半径中心
        int max = Integer.MIN_VALUE;

        /*
         *  以i作为作为回文中点, 找最长的回文串
         *  三目运算符和while循环实现四种情况的判断
         *      - i >= R: 表示i在R外部, 暴力扩展, 此时回文半径至少为1
         *      - i <  R: 表示i在R里面, i的回文半径可能在以C为回文半径的圈内, 也可能不是
         *      - 在while内: 可以进行上面的暴力扩展, 也可以是i在R内, 作为扩不动的情况直接break
         */
        for (int i = 0; i < chars.length; ++i) {
            radius[i] = i >= R ? 1 : Math.min(radius[2*C-i], R-i);
            while (i + radius[i] < chars.length && i - radius[i] > -1) {
                if (chars[i+radius[i]] == chars[i-radius[i]]) {
                    ++radius[i];
                }
                else {
                    break;
                }
            }

            if (i + radius[i] > R) {
                R = i + radius[i];
                C = i;
            }
            max = Math.max(max, radius[i]);
        }

        /*
         * 返回值s字符串的最大回文长度 等于最大回文半径 - 1
         * eg.
         *    1221  ==> #1#2#2#1#   返回值为 5-1 = 4
         *    12321 ==> #1#2#3#2#1# 返回值为 6-1 = 5
         */
        return max - 1;
    }

    public static char []toManacherString(String s) {
        char []chars = new char[s.length() * 2 + 1];
        int index = 0;
        for (int i = 0; i < chars.length; ++i) {
            chars[i] = i % 2 == 0 ? '#' : s.charAt(index++);
        }

        return chars;
    }

    public static void main(String []args) {
        String s1 = "123";
        String s2 = "ab43534c1234321ab";

        System.out.println("s1: " + maxLengthOfPalindrome(s1));
        System.out.println("s2: " + maxLengthOfPalindrome(s2));
    }
}
```