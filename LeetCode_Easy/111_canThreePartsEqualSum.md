# 将数组分成和相等的三个部分
***
#### 2020.07.26

### 问题
>给你一个整数数组 A，只有可以将其划分为三个和相等的非空部分时才返回 true，否则返回 false。                        
形式上，如果可以找出索引 i+1 < j 且满足 A[0] + A[1] + ... + A[i] == A[i+1] + A[i+2] + ... + A[j-1] == A[j] + A[j-1] + ... + A[A.length - 1] 就可以将数组三等分。          

### 示例
>输入：[0,2,1,-6,6,-7,9,1,2,0,1]             
输出：true                  
解释：0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1                   

>输入：[0,2,1,-6,6,7,9,-1,2,0,1]              
输出：false           

>输入：[3,3,6,5,-2,2,5,1,-9,4]               
输出：true                           
解释：3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4          

### 提示
>1.3 <= A.length <= 50000                 
2.-10^4 <= A[i] <= 10^4            

### 代码
```c++
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& A) {
        int sum=accumulate(A.begin(),A.end(),0);
        if(sum%3!=0)
        return 0;
        int flag=0,subsum=0;
        for(int i=0;i<A.size();i++)
        {
            subsum=subsum+A[i];
            if(subsum*3==sum)
            {
                flag++;
                subsum=0;
            }
            if(flag==3)
            return 1;
        }

        return 0;
    }
};
```

### 分析
 - 执行用时：112 ms, 在所有 C++ 提交中击败了58.22%的用户内存消耗：30.6 MB, 在所有 C++ 提交中击败了33.33%的用户。
 - 而这种切分方法也是大家普遍采用的。值得注意的是当计数器出现3即可返回true，因为有可能出现和为0的情况造成最后的计数器大于3。
 - 记录总和除以3的商出现的次数，若可达到三次则可以实现。这道题的题解在求和的过程的函数accumulate函数第一次见，很方便。详解：[accumulate()函数用法](https://www.jianshu.com/p/dd664eae6abc)。

### 优解
#### 1.60ms最低耗时范例
```cpp
static const auto _ = []()
{
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    return nullptr;
}();

class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& A) {
        int sum = std::accumulate(A.begin(), A.end(), 0);
        if (sum % 3 != 0) {
            return false;
        }
        sum /= 3;
        
        // | - | - | - | - | - | - | - | - |
        // |     first     |  mid  |  last |
        //
        // first
        std::size_t i = 0;
        int firstSum = 0;
        for (; i < A.size(); ++i) {
            firstSum += A[i];
            if (firstSum == sum) {
                break;
            }
        }

        if (firstSum != sum) {
            return false;
        }

        // mid
        int midSum = 0;
        for (std::size_t j = i + 1; j + 1 < A.size(); ++j) {
            midSum += A[j];
            if (midSum == sum) {
                return true;
            }
        }

        return false;
    }
};
```

#### 2.12536kb最低存耗范例
```cpp
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& A) {
        int s = 0;
        for(int i = 0;i < A.size();i++)
            s += A[i];
        if(s % 3 != 0) return 0;
        int g = s / 3,h = 0;
        bool f1 = 0,f2 = 0;
        for(int i = 0;i < A.size();i++){
            h += A[i];
            if(h == g) f1 = 1;
            else if(f1 && h == 2 * g) return 1;
        }
        return 0;
    }
};
```
