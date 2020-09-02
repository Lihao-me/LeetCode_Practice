# 数组形式的整数加法
***
#### 2020.09.02

### 问题
>对于非负整数 X 而言，X 的数组形式是每位数字按从左到右的顺序形成的数组。例如，如果 X = 1231，那么其数组形式为 [1,2,3,1]。                 
给定非负整数 X 的数组形式 A，返回整数 X+K 的数组形式。                  

### 示例
>输入：A = [1,2,0,0], K = 34                  
输出：[1,2,3,4]              
解释：1200 + 34 = 1234                            

>输入：A = [2,7,4], K = 181                            
输出：[4,5,5]               
解释：274 + 181 = 455                    

>输入：A = [2,1,5], K = 806            
输出：[1,0,2,1]                            
解释：215 + 806 = 1021              

>输入：A = [9,9,9,9,9,9,9,9,9,9], K = 1                                     
输出：[1,0,0,0,0,0,0,0,0,0,0]                          
解释：9999999999 + 1 = 10000000000                           

### 提示
>1 <= A.length <= 10000                   
	0 <= A[i] <= 9                               
	0 <= K <= 10000                        
	如果 A.length > 1，那么 A[0] != 0             

### 代码
```cpp
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& A, int K) {
        if(K==0)
        return A;
        vector<int>B;
        while(K!=0)
        {
            int temp=K%10;
            B.push_back(temp);
            K=K/10;
        }
        reverse(B.begin(),B.end());
        int a=A.size(),b=B.size();
        
        if(a>=b)
        {
            for(int i=a-1;i>=0;i--)
            {
                int m=A[i];
                if(i>=a-b)
                m=m+B[i-a+b];
                
                if(m<=9)
                A[i]=m;
                else if(m>9&&i>0){
                A[i-1]++;
                A[i]=m-10;}
                else if(m>9&&i==0)
                {
                    A[i]=m-10;
                    A.insert(A.begin(),1);
                }
            }
            return A;
        }
        else
        {
            for(int i=b-1;i>=0;i--)
            {
                int n=B[i];
                if(i>=b-a)
                n=n+A[i-b+a];
                
                if(n<=9)
                B[i]=n;
                else if(n>9&&i>0){
                B[i-1]++;
                B[i]=n-10;}
                else if(n>9&&i==0)
                {
                    B[i]=n-10;
                    B.insert(B.begin(),1);
                }

            }
            return B;
        }

        return A;

    }
};
```

### 分析
 - 执行用时：88 ms, 在所有 C++ 提交中击败了20.79%的用户内存消耗：26.3 MB, 在所有 C++ 提交中击败了73.66%的用户。
 - 这道题开始的思路走的不是很简洁，所以后来试了很多错才把最终的情况分类出来。主要还是将K植入到数组中进行操作。通过插入的方法会更简单一些。      
 
### 优解
#### 1.0ms最短用时
```cpp
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& A, int K) {
        int n = A.size();
        int high = K;

        for(int i = n - 1; i >= 0; i--) {
            A[i] += high;
            high = A[i] / 10;
            A[i] = A[i] % 10;
            if (high == 0) break;
        }

        while(high > 0) {
            A.insert(A.begin(), high % 10);
            high /= 10;
        }

        return A;
    }
};
```

#### 2.最少内存消耗25948kb
```cpp
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& A, int K) {
        vector<int> ans;
        ans.reserve(A.size()+1);
        for(int i = A.size()-1 ; i >=0||K>0; --i)
        {
            K+= (i>=0 ? A[i]:0);
            ans.push_back(K%10);
            K/=10;

        }
        reverse(begin(ans),end(ans));
        return ans;
    }
};
```

#### 3.官方题解逐位相加
```cpp
class Solution {
    public List<Integer> addToArrayForm(int[] A, int K) {
        int N = A.length;
        int cur = K;
        List<Integer> ans = new ArrayList();

        int i = N;
        while (--i >= 0 || cur > 0) {
            if (i >= 0)
                cur += A[i];
            ans.add(cur % 10);
            cur /= 10;
        }

        Collections.reverse(ans);
        return ans;
    }
}

作者：LeetCode
链接：https://leetcode-cn.com/problems/add-to-array-form-of-integer/solution/shu-zu-xing-shi-de-zheng-shu-jia-fa-by-leetcode/
```

#### 4.原数组操作
```cpp
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& A, int K) {
		reverse(A.begin(), A.end());
		int index = 0;
		while (K > 0)
		{
			if (index < A.size())
			{
				K += A[index];
				A[index] = K % 10; // 未越界, 直接赋值
			}
			else
			{
				A.push_back(K % 10); // 越界了, push
			}
			
			K /= 10;
			++index;
		}
		reverse(A.begin(), A.end());
		return A;
    }
};

作者：AnthonyFu
链接：https://leetcode-cn.com/problems/add-to-array-form-of-integer/solution/c-su-du-ji-bai-9991-bu-xu-yao-e-wai-shu-zu-yuan-sh/
```
