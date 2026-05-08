# CF1486B Eastern Exhibition

## 题目描述

你和你的朋友们住在 $n$ 座房子里。每座房子都位于二维平面上的一个整数坐标点。可能有不同的房子位于同一个点。市长要求你为“东方展览会”的建设选址。你需要找出有多少个位置（即整数坐标点），使得所有房子到展览会的距离之和最小。展览会可以建在某个房子所在的位置。两点 $(x_1, y_1)$ 和 $(x_2, y_2)$ 之间的距离为 $|x_1 - x_2| + |y_1 - y_2|$，其中 $|x|$ 表示 $x$ 的绝对值。

## 输入格式

第一行包含一个整数 $t$ $(1 \leq t \leq 1000)$，表示测试用例的数量。

每个测试用例的第一行包含一个整数 $n$ $(1 \leq n \leq 1000)$。接下来的 $n$ 行，每行描述一座房子的位置 $(x_i, y_i)$ $(0 \leq x_i, y_i \leq 10^9)$。

保证所有测试用例中 $n$ 的总和不超过 $1000$。

## 输出格式

对于每个测试用例，输出一个整数，表示展览会可以建设的不同位置的数量。展览会可以建在某个房子所在的位置。

## 输入输出样例 #1

### 输入 #1

```
6
3
0 0
2 0
1 2
4
1 0
0 2
2 3
3 1
4
0 0
0 1
1 0
1 1
2
0 0
1 1
2
0 0
2 0
2
0 0
0 0
```

### 输出 #1

```
1
4
4
4
3
1
```

## 说明/提示

以下是示例测试用例的图片。蓝点表示房子的位置，绿色点表示展览会可能的位置。

![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF1486B/ef8e7107a46bf60bf70b2b89dad798828df776de.png)

第一个测试用例。

![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF1486B/3bfbc5c12bc5661837030d46309064e5728abb33.png)

第二个测试用例。![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF1486B/f4355abdf55b6e2aba57eaba9ac1bd5b3e9a9937.png)

第三个测试用例。![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF1486B/7ac3252595a464db25ea4d6a5a88bb674df5da85.png)

第四个测试用例。![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF1486B/2ad0e39ceaeb8df93cdbdc07468766d61acf71ed.png)

第五个测试用例。![](https://cdn.luogu.com.cn/upload/vjudge_pic/CF1486B/bf530ae677a95adfe7eacb87263816efab51ccdb.png)

第六个测试用例。这里两座房子都位于 $(0, 0)$。

由 ChatGPT 4.1 翻译

## 分析
作为一道最短距离和的模板提。
这类题目的关键是我们要知道一个结论：
**在一维直线中，所有点距离和最小点是它们最中间的那一个点。**
如果在``0~based``索引中，奇数个元素就是``a[n/2]``，偶数个元素就是``a[n/2]``与``a[(n-1)/2]``，n是元素个数。如果是``1~based``索引，奇数个元素就是``a[(n+1)/2]``，偶数个元素就是``a[(n+1)/2]``与``a[n/2]``。
这道题是二维，所以我们要扩展一下，把坐标分别投影到x轴和y轴上，分别计算有多少个最小值点，然后相乘就得到了答案。如果是奇数个就只有一个点，偶数个就是两个中位数区间的所有点。
## 代码
```cpp
#include <bits/stdc++.h>
using namespace std;
int t;
int main()
{
    cin>>t;
    while(t--)
    {
        int n;
        cin>>n;
        vector<int>x(n);
        vector<int>y(n);
        for(int i=0;i<n;i++)cin>>x[i]>>y[i];
        sort(x.begin(),x.end());
        sort(y.begin(),y.end());//对x轴和y轴上投影的点进行排序
        if(n%2)
        {
            cout<<1<<'\n';//如果n是奇数直接输出1
            continue;
        }
        int cnt1=x[n/2]-x[(n-1)/2]+1;
        int cnt2=y[n/2]-y[(n-1)/2]+1;//计算中位数区间间的点个数
        cout<<cnt1*cnt2<<'\n';//相乘得到答案
    }
    return 0;
}
```