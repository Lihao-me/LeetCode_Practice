# 移除元素
***
#### 2020.07.06

### 问题
>给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。               
不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。                                    
元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。                     

### 示例
>给定 nums = [3,2,2,3], val = 3,                            
函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。                     
你不需要考虑数组中超出新长度后面的元素。                    

>给定 nums = [0,1,2,2,3,0,4,2], val = 2,                   
函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。                    
注意这五个元素可为任意顺序。                  
你不需要考虑数组中超出新长度后面的元素。                                    

### 说明
>为什么返回数值是整数，但输出的答案是数组呢?                               
请注意，输入数组是以「引用」方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。                                   
你可以想象内部操作如下:                   
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝                        
int len = removeElement(nums, val);                  
// 在函数里修改输入数组对于调用者是可见的。                                          
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。                  
for (int i = 0; i < len; i++) {                    
    print(nums[i]);                      
}                     

### 代码
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k=nums.size();
        for(int i=nums.size()-1;i>=0;i--)
        {
            if(nums[i]==val)
            {
                k--;
                for(int j=i;j<nums.size()-1;j++)
                {
                    nums[j]=nums[j+1];
                }
      
            }
        }
        return k;
    }
};
```

### 分析
 - 执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户内存消耗：6.3 MB, 在所有 C++ 提交中击败了100.00%的用户。
 - 这道题一方面需要计算“移除”后的数组长度，一方面需要将保留的元素提前，难度不是很大。即使是两个循环，也并没有超时。我看题解上有人将nums.pop_back()，我觉得是没有这个必要的。他可能纯粹
   为了最后计算数组长度方便，也可能是没有理解为什么要在原数组更改元素。
 - 题解中的使用向量容器的erase功能真的很简单，我也是刚刚学习，没能想到这个方法应该反省一下自己的学习。而优解3的partition就更难想了，未曾接触过。具体的特性可见：https://blog.csdn.net/drecik__/article/details/79268840
   
### 优解
#### 1.双指针、单循环（内存最低）
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n =nums.size();
        int start = 0,end = n - 1;
        while(start <= end)
        {
            // cout<<start<<" "<<end<<endl;
            if(nums[end] == val) end--;
            else if(nums[start] == val && nums[end] != val) swap(nums[start++],nums[end--]);   
            else start ++ ;
            
        }
        return start ;
    }
};
```

#### 2.向量容器
```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        vector<int>::iterator p=nums.begin();
        while(p!=nums.end())
        {
            if(*p==val)
            {
                p=nums.erase(p);
            }
            else
            {
                p++;
            }
        }
        return nums.size();
    }
};

作者：zzy-34f
链接：https://leetcode-cn.com/problems/remove-element/solution/jian-dan-guo-guan-by-zzy-34f/
```

#### 3.STL
```c++
int removeElement(vector<int>& nums, int val) {
        return distance(nums.begin(),partition(nums.begin(),nums.end(),[=](const int& a){return a != val;}));
    }

作者：f0rtwist
链接：https://leetcode-cn.com/problems/remove-element/solution/c-stl-yi-xing-by-f0rtwist/
```
