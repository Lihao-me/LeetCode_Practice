# 删除排序数组中的重复项
***
#### 2020.02.13

### 问题
>给定一个排序数组，你需要在原地删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

### 示例
>给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。

>给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素。

>说明:
为什么返回数值是整数，但输出的答案是数组呢?
请注意，输入数组是以“引用”方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。
你可以想象内部操作如下:
// nums 是以“引用”方式传递的。也就是说，不对实参做任何拷贝
int len = removeDuplicates(nums);
// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中该长度范围内的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}

### 代码
```c
int removeDuplicates(int* nums, int numsSize){
    if(numsSize==0)
    return 0;
    int i=0,k;
    for(k=1;k<numsSize;k++){
        if(nums[k]!=nums[i]){
            i=i+1;
            nums[i]=nums[k];
        }
    }
    return i+1;

}
```

### 分析
 - 看到这道题的要求后不难发现输出的数组是在原数组内进行相应长度的截取，则需要我们在原数组内更改重复值且使最终结果按顺序排列。新构一个数组的办法显然
   就不可行了。想了好久之后可以发现一个替换规律，找到和前面的数不相等的数时，其前的数予以替换，直到最终替换完成。
 - 这就又可以想到之前所学的“双指针”了。代码中，i作为一个指针记录需替换的数的位置，k作为另一指针完成循环移动。用时上还比较理想，但内存消耗上就不太理想
   了。执行用时 :24 ms, 在所有 C 提交中击败了83.81%的用户内存消耗 :9.7 MB, 在所有 C 提交中击败了7.95%的用户。
 - 除此之外还有一些其他的方法以供学习，当然暴力求解无论是时间还是内存上消耗太大不作为首选。快慢指针也值得学习,以及“泛型算法”也是不易接触的东西。

单指针：遍历nums中的元素n，与指针对应的元素进行比较，如果不同，则替换初始指针后面的位置为新的元素n
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        
        if(nums.size()==0)    return 0;

        int i=0;   //i做指针，记录需要替换元素的位置 
        for(int n:nums)
        {
            if(n!=nums[i])
            nums[++i]=n;
        }

        return i+1;
    }
};

作者：zrita
```
快慢指针
```c
int removeDuplicates(int* nums, int numsSize){
    if (numsSize <= 1) 
    {
        return numsSize;
    }

    int size = 0;
    int slow = 0;
    int fast = 0;

    for (slow = 0; slow <= numsSize - 2; slow = fast) // 慢的到倒数第二个位置就行了
    {
        for (fast = slow + 1; fast <= numsSize - 1; fast++)
        {
            if (nums[slow] != nums[fast])
            {
                break;
            }
        }

        if (fast <= numsSize - 1) // 排除最后两个是相等的情况
        {
            nums[++size] = nums[fast];
        }
    }

作者：hua-jia-wang-tie-nan
```
常规交换，遇到重复数据就移到末尾，O(n^2)
```c++
int removeDuplicates(vector<int>& nums) {
    int ret = nums.size();
    if (ret < 2)
        return ret;
    for (int i = 1; i < ret;) {
        if (nums[i] == nums[i - 1]) {
            for (int j = i; j < ret - 1; ++j) {
                swap(nums[j], nums[j + 1]);
            }
            --ret;
        } else {
            ++i;
        }
    }
    return ret;
}

作者：nextplan
```
两次单独for循环，第一次计算长度并标记重复项为x，第二次交换重复项，O(n)
```c++
int removeDuplicates(vector<int>& nums) {
    int s = nums.size();
    int ret = s;
    if (ret < 2) return ret;
    
    int x = 0;//升序还是降序
    if (nums[0] <= nums[ret - 1]) x = nums[0] - 1;
    else x = nums[0] + 1;

    for (int i = 1, pre = 0; i < s; ++i) {
        if (nums[i] == nums[pre]) {
            --ret;
            nums[i] = x;
        } else {
            pre = i;
        }
    }

    for (int i = 1, xp = -1; i < s; ++i) {
        if (nums[i] != x && xp > 0) {
            swap(nums[i], nums[xp++]);
        } else if (nums[i] == x && nums[i] != nums[i - 1]) {
            xp = i;
        }
    }
    return ret;
}

作者：nextplan
```
泛型算法
```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        auto iter=unique(nums.begin(),nums.end());      //重排容器
        nums.erase(iter,nums.end());    //删除重复元素
        return nums.size();     
    }
};

作者：xuejian
