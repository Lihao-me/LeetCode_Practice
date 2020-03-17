# 移动零
***
#### 2020.03.17

### 问题
>给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

### 示例
>输入: [0,1,0,3,12]     
输出: [1,3,12,0,0]      

### 说明
>1.必须在原数组上操作，不能拷贝额外的数组。    
2.尽量减少操作次数。     

### 思路
>检查数组，遇见0后与其最近的非0数交换。

### 代码
```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n=nums.size(),j=0;
        for(int i=0;i<n;i++){
            if(nums[i]!=0&&nums[j]==0){
                nums[j]=nums[i];
                j++;
                nums[i]=0;
            }
            if(nums[i]!=0&&nums[j]!=0)
            j++;
        }
    }

};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了98.31%的用户内存消耗 :10.4 MB, 在所有 C++ 提交中击败了5.09%的用户。双下标的思路非常好想。由于只能在原
   数组上操作，所以不能重新申请数组进行实现。而且原非0数的顺序不能改变，所以排序也是行不通的。
 - 通过0与其后面最近的非0数交换位置，实现整体的移动。
 
 使用迭代器
 ```c++
 遍历数组，只要遇到0则删除，count加1（计0的个数），最后在末尾补上count个0即可。因为使用了迭代器，要注意的是避免迭代器失效。
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int count = 0;
        for(auto it=nums.begin(); it!=nums.end(); ){
            if(*it == 0){
                it = nums.erase(it);
                count ++; 
            }
            else{++it;}
        }
        nums.insert(nums.end(), count, 0);
    }
};

作者：xia-mo-ji-yu
```
