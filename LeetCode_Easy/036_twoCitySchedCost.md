# 两地调度
***
#### 2020.02.27

### 问题
>公司计划面试 2N 人。第 i 人飞往 A 市的费用为 costs[i][0]，飞往 B 市的费用为 costs[i][1]。
返回将每个人都飞到某座城市的最低费用，要求每个城市都有 N 人抵达。

### 示例
>输入：[[10,20],[30,200],[400,50],[30,20]]
输出：110
解释：
第一个人去 A 市，费用为 10。
第二个人去 A 市，费用为 30。
第三个人去 B 市，费用为 50。
第四个人去 B 市，费用为 20。
最低总费用为 10 + 30 + 50 + 20 = 110，每个城市都有一半的人在面试。

>提示：
1.1 <= costs.length <= 100
2.costs.length 为偶数
3.1 <= costs[i][0], costs[i][1] <= 1000

### 思路
>先假设所有人都去A地并计算费用，计算一个人去A、B两地的费用差额，将差额进行排序，用都去A地的费用减去排序的较大一半即得最低费用。

### 代码
```c++
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        vector<int>a;
        int sum=0;
        for(int i=0;i<costs.size();i++){
            a.push_back(costs[i][0]-costs[i][1]);
            sum=sum+costs[i][0];
        }
        sort(a.begin(),a.end());
        int suml=0;
        for(int j=costs.size()-1;j>=costs.size()/2;j--){
            suml=suml+a[j];
        }
        return sum-suml;
        
    }
};
```

### 分析
 - 这道题的一般想法是判断哪一半的人留在A，另一半去B，但由于是二维数组，执行起来非常复杂。我也是参考了题解的贪心法，认为将其看作数学问题进行思考最
   简便。我们只需要笼统计算费用之和，通过申请一个一维数组进行排序就非常方便了。
 - 执行用时 :8 ms, 在所有 C++ 提交中击败了56.52%的用户内存消耗 :10.2 MB, 在所有 C++ 提交中击败了14.29%的用户。而根据网友的解法学习了qsort这一
   C语言下的排序函数。https://blog.csdn.net/zhao888789/article/details/79186619 除此之外还有数学极值法，这就需要用到数学知识了。
 
 qsort函数
 ```c
 int cmp(const *a,const *b){
    int *ap = *(int **)a;       
    int *bp = *(int **)b;
    int t1 = ap[0]-ap[1];
    int t2 = bp[0]-bp[1];
    return t1>t2?1:-1;
}
int twoCitySchedCost(int** costs, int n, int* col){
    qsort(costs,n,sizeof(int**),cmp);
    int sum=0;
    for(int i=0;i<n;i++){
        // printf("(%d,%d)\n",costs[i][0],costs[i][1]);
        if(i<n/2) sum+=costs[i][0];
        else sum+=costs[i][1];
    }
    return sum;
    
}

作者：JUNHAO_
```

数学极值法
[图片1](https://github.com/Lihao-me/Pictures/blob/master/twoCitySchedCost_1.png)
[图片2](https://github.com/Lihao-me/Pictures/blob/master/twoCitySchedCost_2.png)
```c++
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        int ans=0;
        vector<int> diff;
        for(int i=0;i<costs.size();i++){
            diff.push_back(costs[i][0]-costs[i][1]);
        }
        sort(diff.begin(),diff.end());
        for(int i=0;i<diff.size();i++){
            ans = ans + costs[i][1];
            if(i<diff.size()/2){
                ans=ans+diff[i];
            }
        }
        return ans;
        
    }
};

作者：yi-di-ji-mao-5
```

最优策略
有最优策略，先把2N个人随意分成A、B两个集合，每个集合N个人。A集合去A地，B集合去B地。然后对于A集合中每个人ai，到B集合中遍历每个人bj，
看是否能互换位置，让整体费用更低。如果有则换位置，没有检查下一个。全部换完则是最优解。换的条件是ai去A的费用+bj去B的费用，要大于ai去
B的费用+bj去A的费用。这样bj与ai互换整体费用会最低。时间复杂度O(n^2)
```c++
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        vector<int> va;
        vector<int> vb;
        for(int i=0;i<costs.size();++i)
        {
            if(i%2==0)
                va.push_back(i);
            else
                vb.push_back(i);
        }
        for(int i=0;i<va.size();++i)
        {
            for(int j=0; j<vb.size();++j)
            {
                int aindex=va[i];
                int bindex=vb[j];
                int acost = costs[aindex][0]+costs[bindex][1];
                int bcost = costs[aindex][1]+costs[bindex][0];
                if(acost>bcost)
                {
                    swap(va[i],vb[j]);
                }
            }
        }
        int sum = 0;
        for(auto i : va)
        {
            sum+=costs[i][0];
        }
        for(auto i : vb)
        {
            sum+=costs[i][1];
        }
        return sum;
    }
};

作者：owenzzz
