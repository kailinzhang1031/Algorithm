# BrainMan (HW2, POJ1804) #

## 关键知识点： Greedy, Merge Sort ##

### 题目描述： ###
对于一个序列，通过两两交换的方式使之成为降序排列的序列，求最小的交换次数。

### 题目分析： ###
对于一个序列S = S(a<sub>1</sub>, a<sub>2</sub>, a<sub>3</sub>, …, a<sub>n</sub>)，假设其逆序对有<a<sub>1</sub> , b<sub>1></sub>，<a<sub>2</sub> b<sub>2</sub>>，<a<sub>3</sub>, 
b<sub>3</sub>>，…，<a<sub>k</sub>, b<sub>k</sub>>，

其中对于任何一个逆序对<a<sub>i</sub>, b<sub>i</sub>>，满足a<sub>i</sub> > b<sub>i</sub>。

我们先对所有的逆序对进行排序，排序后的逆序对满足以下关系：

按照a<sub>j</sub>降序排序，当a<sub>j</sub>相等时，按b<sub>j</sub>升序排序，

按照a<sub>j</sub>降序排序的目的是，先尽可能让较大的元素进行交换，并排到序列S的后面，

按b<sub>j</sub>升序排序的目的是，先尽可能让较小的元素进行交换，并排到序列S的前面。

这样可以实现局部的最优解。

我们也可以理解为，正是按照逆序对进行交换，可以在每次交换后得到局部最优解，所以最少交换次数即为逆序对的个数。

所以，对于一个序列S，若要求通过两两交换的方式实现升序排序，最少的次数即为这个序列的逆序数。在这种情况下，我们根据前面叙述的规则，按逆序对进行交换即可得到升序序列。

可以通过归并排序求逆序对的个数，大致思路如下：

将一个序列分成左右两个部分，对于右子序列的每个元素q，求出左子序列中大于q的元素p，计数器counter++。但是，当每次求出一个p后，我们进行了一次归并排序，所以不会有重复。归并排序自下而上递归时，计数器counter将记录所有p的个数。


### 代码实现： ###
```c++
#include <stdio.h>
#include <limits.h>

long long Count = 0;
const int MAX_N = 1000 + 10;
long long A[MAX_N], L[MAX_N], R[MAX_N];

void Merge(long long A[], int p, int q, int r)
{
    int i, j, k;

    int n1 = q - p + 1;
    int n2 = r - q;

    for (int i = 0; i < n1; ++i)
        L[i] = A[p + i];
    for (int j = 0; j < n2; ++j)
        R[j] = A[q + j + 1];

    i = j = 0;
    k = p;
    L[n1] = INT_MAX;
    R[n2] = INT_MAX;

    while (k <= r)
    {
        if (L[i] > R[j])
        {
            A[k++] = R[j++];
            Count += n1 - i;
        }
        else
            A[k++] = L[i++];
    }
}

void Merge_Sort(long long A[], int p, int q)
{
    int r = (p + q) / 2;

    if (p < q)
    {
        Merge_Sort(A, p, r);
        Merge_Sort(A, r + 1, q);
        Merge(A, p, r, q);
    }
}

int main()
{
    int n;
    int Scenario = 0;

    scanf("%d", &n);
    for (int i = 0; i < n; ++i)
    {
        int Num;

        scanf("%d", &Num);

        Count = 0;
        Scenario++;
        for (int i = 0; i < Num; ++i)
            scanf("%lld ", &A[i]);

        Merge_Sort(A, 0, Num - 1);
        printf("Scenario #%d:\n%lld\n\n", Scenario, Count);
    }

    return 0;
}
```

### 引用： ###
题目：http://poj.org/problem?id=1804

源代码：https://www.cnblogs.com/tallisHe/p/4012370.html
