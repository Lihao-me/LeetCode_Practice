#  调整数组顺序使奇数位于偶数前面
***
#### 2020.04.05

### 问题
>输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数位于数组的前半部分，所有偶数位于数组的后半部分。

### 示例
>输入：nums = [1,2,3,4]           
输出：[1,3,2,4]                  
注：[3,1,2,4] 也是正确的答案之一。               

### 提示
>1.1 <= nums.length <= 50000
2.1 <= nums[i] <= 10000

### 代码
```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        vector<int>res;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]%2!=0)
            res.push_back(nums[i]);
        }
        for(int j=0;j<nums.size();j++)
        {
            if(nums[j]%2==0)
            res.push_back(nums[j]);
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :44 ms, 在所有 C++ 提交中击败了22.26%的用户内存消耗 :18.9 MB, 在所有 C++ 提交中击败了100.00%的用户。记得还是初学C语言的时候做过
   这道题，现在想想还是很怀念啊。那既然是一道非常简单的题，我就尽量运用后来学到的知识，在尽量少的时空下完成。
 - 但是结果好像不是很理想。主要是这两个循环已知不好突破。看了题解之后恍然大悟，竟然忘了双指针。那就可以实现一次遍历了，甚至还不用申请多余的
   数组空间。双栈分离的思路也很棒。双指针的那个动图很清晰：
   https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/solution/ti-jie-shou-wei-shuang-zhi-zhen-kuai-man-shuang-zh/



双指针
```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int left = 0, right = nums.size() - 1;
        while (left < right) {
            if ((nums[left] & 1) != 0) {
                left ++;
                continue;
            }
            if ((nums[right] & 1) != 1) {
                right --;
                continue;
            }
            swap(nums[left++], nums[right--]);
        }
        return nums;
    }
};

作者：huwt
```

快慢指针
```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int low = 0, fast = 0;
        while (fast < nums.size()) {
            if (nums[fast] & 1) {
                swap(nums[low], nums[fast]);
                low ++;
            }
            fast ++;
        }
        return nums;
    }
};

作者：huwt
```

双栈分离
```c++
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        stack<int> a;
        stack<int> b;
        vector<int> res;
        for(int i=0;i<nums.size();i++)
        {
            if(nums[i]%2==1)
            a.push(nums[i]);
            else
            b.push(nums[i]);
        }
        static int j=0;
        while(!a.empty())
        {
           res.push_back(a.top());
           a.pop();
        }
        while(!b.empty())
        {
           res.push_back(b.top());
           b.pop();
        }
        return res;
    }
};

作者：dou-bi-5
