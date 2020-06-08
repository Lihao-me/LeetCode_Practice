# 旋转数组
***
#### 2020.06.08

### 问题
>给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。                 

### 示例
>输入: [1,2,3,4,5,6,7] 和 k = 3                    
输出: [5,6,7,1,2,3,4]                    
解释:                            
向右旋转 1 步: [7,1,2,3,4,5,6]                          
向右旋转 2 步: [6,7,1,2,3,4,5]                           
向右旋转 3 步: [5,6,7,1,2,3,4]                         

>输入: [-1,-100,3,99] 和 k = 2                        
输出: [3,99,-1,-100]                         
解释:                            
向右旋转 1 步: [99,-1,-100,3]                    
向右旋转 2 步: [3,99,-1,-100]                              

### 说明
>1.尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。                    
2.要求使用空间复杂度为 O(1) 的 原地 算法。                

### 代码
```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n=nums.size();
        reverse(nums.begin(),nums.end()-k%n);
        reverse(nums.end()-k%n,nums.end());
        reverse(nums.begin(),nums.end());
    
    }
};
```

### 分析
 - 执行用时 :24 ms, 在所有 C++ 提交中击败了7.36%的用户内存消耗 :9.9 MB, 在所有 C++ 提交中击败了5.55%的用户。本来是暴力的旋转k次，但是超时
   了。所以参考了网友的解法，感觉很巧妙。旋转三次。
   
### 优解
#### 1.环装替换
```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        for(int start=0,count=0;count<nums.size();++start)
        {
            int rev1=nums[start],rev2,next=start;
            do{
                next=(next+k)%nums.size();
                rev2=nums[next];
                nums[next]=rev1;
                rev1=rev2;
                ++count;
            }while(start!=next);
        }
    }
};

作者：Quinn-code
```

#### 2.STL
```c++
void rotate(vector<int>& nums, int k) {
        if(nums.size()<2 || (k %= nums.size())==0) return;
        ::rotate(nums.begin(),nums.begin()+distance(nums.begin(),nums.end())-k,nums.end());
    }

作者：f0rtwist
```

#### 3.新建数组
```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        vector<int> nums2(nums);
        int len=nums.size();

        for(int i=0;i<len;i++)
        {
            nums[(i+k)%len]=nums2[i];
        }
       
    }
};`

作者：zrita
```
