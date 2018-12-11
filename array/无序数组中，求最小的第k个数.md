# 无序数组中，求最小的第k个数


## 荷兰国旗算法


利用荷兰国旗的算法，时间复杂度的数学期望为O(n)，时间复杂度最差为O(n^2)

```java
public class Netherlands {

    public static void getMinKth(int []a, int k) {
        partition(a, 0, a.length - 1, k - 1);
    }

    public static void partition(int []a, int lo, int hi, int j) {
        if (lo >= hi) {
            return;
        }

        int lt = lo - 1;
        int  i = lo;
        int gt = hi + 1;
        int pivotValue = a[lo];

        while (i < gt) {
            if (a[i] == pivotValue) {
                ++i;
            }
            else if (a[i] < pivotValue) {
                swap(a, i++, ++lt);
            }
            else {
                swap(a, i, --gt);
            }
        }
        if (j <= gt - 1 && j >= lt + 1) {
            return ;
        }
        else if (j < lt + 1){
            partition(a, lo, lt, j);
        }
        else {
            partition(a, gt, hi, j);
        }
    }

    private static void swap(int []a, int i, int j) {
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    public static void main(String []args) {
        int []a = new int[] {
                4, 1, 9, 5, 3, 8, 7, 2, 6, 3, 1, 2
        };
        getMinKth(a, 8);
        System.out.println(Arrays.toString(a));
    }
}
```



## BFPRT算法

时间复杂度是严格的O(n)


```
定义算法流程为: bfprt(int []a, int k)
1. 将数据分组 下标0~4一组，下标5~9一组，...，最后不满5个的数据为一组(时间复杂度O(n))
2. 小组内排序(时间复杂度O(1)* n/5 = O(n))
3. 拿出每个小组内的中位数，构成一个新数组，数组大小为n/5
4. 求新数组中的bfprt(a, k);

bfprt相较于荷兰国旗算法，选取pivot的位置是有意义的

小   [每一列为新分的小数组]
|    *    x    *    *    x
|    *    x    *    *    x   
|    a    b    c    d    e   --> 每个小数组的上中位数组成新数组
|    x    x    x    x    x
|    x    x    x    x    x
大   1    2    3    4    5  

新生成的数组大小为N/5，假设数组排序为:
  a < c < d < e < b
那么在大数组中，*代表的数值均是小于d的，
即有10n/3的数组是小于d的，最多有10n/7大于d的。
最终在选取d为新的分界点的时候，可以去掉10n/3的数据量。

时间复杂度为，可证明为收敛于O(n):
T(n) = T(n/5) + T(n/7) + O(n) 
     => O(n)
```


```java
public class BFPRT {
    public static void bfprt(int[]a, int k) {
        select(a, 0, a.length - 1, k-1);
        System.out.println(Arrays.toString(a));
    }

    public static int select(int []a, int lo, int hi, int i) {
        if (lo == hi) {
            return a[lo];
        }
        int pivotValue   = medianOfMedians(a, lo, hi);
        int []pivotRange = partition(a, lo, hi, pivotValue);
        if (i <= pivotRange[1] && i >= pivotRange[0]) {
            return a[i];
        }
        else if (i < pivotRange[0]) {
            return select(a, lo, pivotRange[0] - 1, i);
        }
        else {
            return select(a, pivotRange[1] + 1, hi, i);
        }
    }

    public static int medianOfMedians(int []a, int lo, int hi) {
        int counts = hi - lo + 1;
        int offset = counts % 5 == 0 ? 0 : 1;
        int []medianArray = new int[counts / 5 + offset];
        int loI = 0;
        int hiI = 0;

        for (int i = 0; i < medianArray.length; ++i) {
            loI = lo + i * 5;
            hiI = loI + 4;
            medianArray[i] = getMedian(a, loI, Math.min(hiI, hi));
        }

        return select(medianArray, 0, medianArray.length - 1, medianArray.length / 2);
    }

    public static int []partition(int []a, int lo, int hi, int pivotValue) {
        int lt = lo - 1;
        int  i = lo;
        int gt = hi + 1;

        while (i < gt) {
            if (a[i] == pivotValue) {
                i++;
            }
            else if (a[i] < pivotValue) {
                swap(a, i++, ++lt);
            }
            else {
                swap(a, i, --gt);
            }
        }

        return new int[] { lt+1, gt-1 };
    }

    public static int getMedian(int []a, int lo, int hi) {
        insertionSort(a, lo, hi);

        int sum = lo + hi;
        int mid = (sum / 2) + (sum % 2);
        return a[mid];
    }

    public static void insertionSort(int []a, int lo, int hi) {

        for (int i = lo + 1; i <= hi; ++i) {
            for (int j = i; j > lo; --j) {
                if (a[j-1] < a[j]) {
                    break;
                }
                else {
                    swap(a, j-1, j);
                }
            }
        }
    }

    public static void swap(int []a, int i, int j) {
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

    public static void main(String []args) {
        int []a = new int[] {
                5, 1, 9, 4, 3, 8, 7, 2, 6, 3, 1, 2
        };
        bfprt(a, 8);
    }
}
```