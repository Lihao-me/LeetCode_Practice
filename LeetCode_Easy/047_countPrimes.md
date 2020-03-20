# 计数质数
***
#### 2020.03.20

### 问题
>统计所有小于非负整数 n 的质数的数量。   

### 示例
>输入: 10      
输出: 4         
解释: 小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。        

### 思路
>从目标数之前的所有数中依次查找，如果是质数则数量加1。

### 代码
```c
int judge(int num){
    if(num<2)
    return 0;
    if(num==2)
    return 1;
    for(int i=2;i*i<=num;i++){
        if(num%i==0)
        return 0;
        else
        continue;
    }
    return 1;
}
int countPrimes(int n){
    if(n<3)
    return 0;
    int number=0;
    for(int i=1;i<n;i++){
        number=number+judge(i);
    }
    return number;
}
```

### 分析
 - 执行用时 :712 ms, 在所有 C 提交中击败了12.81%的用户内存消耗 :5.1 MB, 在所有 C 提交中击败了100.00%的用户。之前做过判断质数的问题，而这道
   题只是稍微增加了计数的难度。依旧是暴力解法，显然时间上不是很如意。
 - 出于对超时的考虑在i*i<=num上已经对其时间缩短了一半。是时候学习更高明的解法了。
 
埃式筛法       
从2开始作为基数，筛去其后基数的倍数。筛完后下一个剩余的数作为基数。
每次作为基数的数即是素数（除1外没有小于自身的因数）
```c++
void Eratosthenes(int maxn)
{
    int pc = 0;
    memset(vis, 0, sizeof(vis));
    memset(prime, 0, sizeof(prime));
    int m = sqrt(maxn + 0.5);
    for(int i = 2; i <= m; i++)
    {
        if(!vis[i])
        {
            prime[pc ++] = i;
            for(int j = i * i; j <= maxn; j += i)
                vis[j] = 1;
        }
    }
}
```
内循环可以看做是调和极数求和，数量级为O(logN)
总的时间复杂度在O(NlogN)

欧式筛法           
埃式筛法做了很多重复的工作，如6在2和3做基数时都被筛了一次。
欧式筛法则是一个线性时间复杂度的筛法，不再仅以素数为基数，而是以所有的数为基数：
```c++
void Euler(int maxn)
{
    int pc = 0;
    memset(vis, 0, sizeof(vis));
    memset(prime, 0, sizeof(prime));

    for(int i = 2; i < maxn; i++)
    {
        if(!vis[i])
        {
            prime[pc ++] = i;
        }
        vis[i] = pc;
        for(int j = 0; j < pc; j++)
        {
            if(i * prime[j] > maxn) // 当乘积超过范围时结束本轮
                break;
            vis[i * prime[j]] = 1;
            if(i % prime[j] == 0)   // 当基数是某个已知素数的倍数，结束本轮
                break;
        }
    }
}
```
初始时素数列表为空，从2开始枚举
对于每一个基数，将其与已有素数按序相乘，筛去乘积
当乘积超过范围时结束本轮
当如果基数是某个已知素数的倍数，结束本轮
这样每个合数只会被枚举一次，并且一定会被枚举到，证明如下：
对于合数 k=m∗p1,p1 是其最小质因子，k 只会在 i=m,prime[j]=p1 时被枚举到
否则假设 k=n∗p2 且 p2>p1，有n<m，则 p1|n，那么在 i=n,prime[j]=p1 时便会结束，不会到 p2
欧式筛法的时间复杂度只有O(N)，目标范围越大相较于埃式筛法的优势越明显。
作者：biggrass      

线性筛法              
普通的筛法相信大家很容易明白，但它还有优化的空间
普通筛法的冗余性在于，一个数可能被多个质因子筛到
如6被2、3筛，12也被2、3筛，这样的情况，导致普通筛法的复杂度无法到达线性
那么怎么避免这种情况呢？
很简单，保证每个数，只会被它最小的质因子筛掉即可。
在下面的代码中，体现这一条的是
if (i % prime[j] == 0) break;
也就是说，假设30要被筛掉，30=235，则30只在当i=15，取到质数2时被筛掉
原因留给读者慢慢思考
网上也有很多线性筛的资料，这里就不赘述
由于线性筛中每个数只会被筛一次，所以复杂度是O(n)的
代码     
```c++
class Solution {
public:
    int countPrimes(int n) {
        int prime[n+1]; //存放质数
        int label[n+1]; //标记数组
        memset(label, 0, sizeof(label));
        int m = 0;
        for (int i = 2; i < n; ++i) {
            if (label[i] == 0)
                prime[m++] = i;
            for (int j = 0; j < m; ++j) {
                if (i * prime[j] >= n)
                    break;
                label[i * prime[j]] = 1;
                if (i % prime[j] == 0)
                    break;
            }    
        }
        return m;
    }
};
作者：Hongda Jiang@PKU
```

动态规划            
后一个数如果不是质数，它一定可以被之前的质数整除
如果他是一个质数，那么他的因数一定在 2 -> sqrt(n) 之间
后面的因数可以借助之前的因数判断
代码
```c++
class Solution {
public:
    int countPrimes(int n) {
        vector<int> ret;
        ret.push_back(2);
        ret.push_back(3);
        ret.push_back(5);
        ret.push_back(7);
        ret.push_back(11);

        if(n == 0 || n == 1){
            return 0;
        } else if(n <= 11) {
            int i = 0;
            
            for(int j = 0 ; j < ret.size(); j++) {
                if(ret[j] < n) {
                    i++;
                } else {
                    break;
                }
            }
            return i;
        }

        bool is = true;

        for(int k = 12; k < n ; k++) {
            is = true;

            for (int i = 0; i < ret.size(); i++) {
                if( ret[i] * ret[i] > k ) {
                    break;
                }

                if(k % ret[i] == 0 ) {
                    is = false;
                    break;
                }
            }

            if(is) {
                //cout << k <<endl;;
                ret.push_back(k);
            }
        }

        return ret.size();
    }
};
作者：wangchaotank
