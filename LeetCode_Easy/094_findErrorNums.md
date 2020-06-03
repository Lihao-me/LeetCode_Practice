# 简单的错误集合
***
#### 2020.06.04

### 问题
>集合 S 包含从1到 n 的整数。不幸的是，因为数据错误，导致集合里面某一个元素复制了成了集合里面的另外一个元素的值，导致集合丢失了一个整数并且有一个元素重复。
给定一个数组 nums 代表了集合 S 发生错误后的结果。你的任务是首先寻找到重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。     

### 示例
>输入: nums = [1,2,2,4]                                     
输出: [2,3]        

### 注意
>1.给定数组的长度范围是 [2, 10000]。                      
2.给定的数组是无序的。                 

### 代码
```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int>res;
        sort(nums.begin(),nums.end());
        int a=-1,b=-1;
        for(int i=0;i<nums.size()-1;i++)
        {
            if(nums[i]==nums[i+1]){
            a=nums[i];}
            if(nums[i+1]-nums[i]>1)
            b=nums[i]+1;
            if(a!=-1&&b!=-1)
            break;
        }
        if(a!=-1)
        res.push_back(a);
        if(b!=-1)
        res.push_back(b);
        if(res.size()<2)
        {
            if(nums[0]!=1)
            res.push_back(1);
            else
            res.push_back(nums.size());
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :116 ms, 在所有 C++ 提交中击败了28.83%的用户内存消耗 :20.3 MB, 在所有 C++ 提交中击败了100.00%的用户。用这种暴力的方法提交了很多次
   试了很多错，才把判断条件补全。
   
### 优解
#### 1.异或
···c++
class Solution {
public:
	vector<int> findErrorNums(vector<int>& nums) {
		int n = nums.size();
		int sum = 0, xor1 = 0, xor2 = 0;
		int dup = -1, mis = 1;
		vector<int> ans(2);
		for (int i = 0; i < n; ++i) {
			sum ^= (i + 1) ^ nums[i];
		}
		int t = sum & -sum; //将sum二进制表示下除最靠右的1保留外，其余都置为0
		for (int i = 1; i < n + 1; ++i) {
			if (t & i)
				xor1 ^= i;
			else
				xor2 ^= i;
		}
		for (int i = 0; i < n; ++i) {
			if (t & nums[i])
				xor1 ^= nums[i];
			else
				xor2 ^= nums[i];
		}
		int count = 0;
		for (int i = 0; i < n; ++i) {
			if (nums[i] == xor1)
				count++;
		}
		if (count == 0) { dup = xor2; mis = xor1; }
		else { dup = xor1; mis = xor2; }

		ans[0] = dup; ans[1] = mis;
		return ans;

作者：feng-feng-19
```

#### 2.哈希表
```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        vector<int> vec(2,0);//存放最后结果
        map<int,int> map1;//存放出现次数
        for(int i=1;i<=nums.size();i++){
            map1[i]+=1;//1~n之间的数先初始化设为1
            map1[nums[i-1]]+=1;//累加nums中出现的数的次数
        }
        for(int i=1;i<=nums.size();i++){
            if(map1[i]==3) vec[0]=i;//重复的数的次数为3
            else if(map1[i]==1) vec[1]=i;//缺失的数的次数为1
        }
        return vec;
    }
};

作者：llllllll-9
```

#### 3.位运算
```c++
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int sum = 0;
        int n = 0;
        for (int i = 0; i < nums.size(); ++i) {
            n ^= (i + 1) ^ nums[i];
            sum += nums[i];
        }
        int t = n & -n;
        int n1 = 0;
        int n2 = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if ((i + 1) & t) {
                n1 ^= i + 1;
            } else {
                n2 ^= i + 1;
            }
            if (nums[i] & t) {
                n1 ^= nums[i];
            } else {
                n2 ^= nums[i];
            }
        }
        int k = nums.size();
        int s = (k + 1) * k >> 1;
        if (n1 > n2) {
            n1 ^= n2;
            n2 ^= n1;
            n1 ^= n2;
        }
        if (sum > s) return {n2, n1};
        return {n1, n2};
    }
};

作者：da-li-wang
```
