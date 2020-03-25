# 比特位计数
***
#### 2020.03.25

### 问题
>给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

### 示例
>输入: 2     
输出: [0,1,1]       

>输入: 5        
输出: [0,1,1,2,1,2]       

### 进阶
>	给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
	要求算法的空间复杂度为O(n)。
	你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。

### 思路
>对于所有的数字，只有两类：
奇数：二进制表示中，奇数一定比前面那个偶数多一个 1，因为多的就是最低位的 1。     
偶数：二进制表示中，偶数中 1 的个数一定和除以 2 之后的那个数一样多。
因为最低位是 0，除以 2 就是右移一位，也就是把那个 0 抹掉而已，所以 1 的个数是不变的。

### 代码
```c++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int>res(num+1);
        res[0]=0;
        for(int i=1;i<=num;i++){
            if(i%2!=0)
            res[i]=res[i-1]+1;
            else
            res[i]=res[i/2];
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :8 ms, 在所有 C++ 提交中击败了67.89%的用户内存消耗 :7.7 MB, 在所有 C++ 提交中击败了100.00%的用户。上述思路是看了别人的解体方法
   后想出来的，不得不说这个规律是以前不知道的。用暴力的方法，对每一个数字进行位运算的确可行/但看到进阶后我觉得还有简化的方法，没想到能够如此
   简单。

动态规划
首先须知道十进制数如何转化二进制，一句话来说就是“除二取余，倒序排列”。
题目要求1的个数，令F(n)表示十进制数字n转化为二进制后1的个数。
结合动态规划思想来看，可以得出
状态转移公式：F(n) = F(n/2) + n%2
边界：F(0) = 0
代码如下：
```c++
vector<int> countBits(int num) {
        vector<int> a;
        for(int i=0;i<=num;i++){
            if(i==0){
                a.push_back(0);
            }else{  
                int current = a.at(i/2) + i%2;
                a.push_back(current);
            }
        }
        return a;
    }

作者：rosamondtao
```

用一个mask掩码每8位存储1的个数，最后对这个32位整型每8位叠加输出
```c++
class Solution {
public:
    vector<int> countBits(int num) {
        vector<int> ret;
        for(int i=0;i<=num;i++)
        {
            if(i<=1)
                ret.push_back(i);
            else 
                ret.push_back(countBit(i));
        }
        return ret;
    }
    int countBit(int num)
    {
        int mask=0x01|0x01<<8|0x01<<16|0x01<<24,bits=0;

        bits+=mask&(num>>0);
        bits+=mask&(num>>1);
        bits+=mask&(num>>2);
        bits+=mask&(num>>3);
        bits+=mask&(num>>4);
        bits+=mask&(num>>5);
        bits+=mask&(num>>6);
        bits+=mask&(num>>7);
        return (bits&0xff)+((bits>>8)&0xff)+((bits>>16)&0xff)+((bits>>24)&0xff);
    }
};

作者：bant-2
