# 寻找数组的中心索引
***
#### 2020.04.27

### 问题
>给定一个整数类型的数组 nums，请编写一个能够返回数组“中心索引”的方法。                
我们是这样定义数组中心索引的：数组中心索引的左侧所有元素相加的和等于右侧所有元素相加的和。                    
如果数组不存在中心索引，那么我们应该返回 -1。如果数组有多个中心索引，那么我们应该返回最靠近左边的那一个。               

### 示例
>输入:                  
nums = [1, 7, 3, 6, 5, 6]                 
输出: 3                 
解释:                
索引3 (nums[3] = 6) 的左侧数之和(1 + 7 + 3 = 11)，与右侧数之和(5 + 6 = 11)相等。                    
同时, 3 也是第一个符合要求的中心索引。                     

>输入:                
nums = [1, 2, 3]                   
输出: -1                   
解释:                 
数组中不存在满足此条件的中心索引。          

### 说明
>1.nums 的长度范围为 [0, 10000]。
2.任何一个 nums[i] 将会是一个范围在 [-1000, 1000]的整数。

### 代码
```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
    int n=nums.size();
    int suml=0,sumt=0;
        for(int i=0;i<n;i++)
        {
         sumt += nums[i];
        }
        for(int j=0;j<n;j++)
        {
              if(suml*2==sumt-nums[j])
              return j;

         suml += nums[j];
         }
    return -1;
    }
};
```

### 分析
 - 执行用时 :52 ms, 在所有 C++ 提交中击败了30.57%的用户内存消耗 :29.9 MB, 在所有 C++ 提交中击败了6.67%的用户。虽然做出来了，但不是很理想。
   应该还有更高效的办法。
   
前缀和
```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        //计算整个数组数之和
        int sum = accumulate(nums.begin(), nums.end(), 0);
        int leftSum = 0;
        for (int i = 0; i < nums.size(); i++) {
            if(leftSum == sum - leftSum - nums[i]){
                return i;
            }
            //前缀和
            leftSum += nums[i];
        }
        return -1;
    }
};

作者：chuang-bian-gu-shi
```

```c++
class Solution{
	public:
	    int pivotIndex(vector<int>& nums) {
	    	int sum=0,tmp=0;
        	for(int i=0;i<nums.size();++i)sum+=nums[i];
        	for(int i=0;i<nums.size();++i){
        		if(sum-nums[i]==2*tmp)return i;
        		tmp+=nums[i];
			}
			return -1;
    	} 
};

作者：yzwz
```

双指针
```c++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        if(nums.empty()) return -1;
        int sum_left = 0;
        int sum_right = 0;
        for(vector<int>::iterator i = nums.begin(); i != nums.end(); i++){
            
            if(i != nums.begin()){
                sum_left += *(i-1);
                sum_right -= *i;
            }
            if(i == nums.begin()){
                for(vector<int>::iterator k = i + 1; k < nums.end(); k++){
                    sum_right += *k;
                }
            }
            
            if(sum_left == sum_right){
                return i - nums.begin();
            }
        }
        return -1;
    }
};

作者：lao-teng-ye-yao-jia-you-ya
