# 最小的k个数
***
#### 2020.04.06

### 问题
>输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

### 示例
>输入：arr = [3,2,1], k = 2                   
输出：[1,2] 或者 [2,1]            

>输入：arr = [0,1,2,1], k = 1                     
输出：[0]                       

### 限制
>1.0 <= k <= arr.length <= 10000            
2.0 <= arr[i] <= 10000              

### 思路
>将数组由小到大排列，取前k个即可。

### 代码
```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        sort(arr.begin(),arr.end());
        vector<int>res;
        for(int i=0;i<k;i++)
        {
            res.push_back(arr[i]);
        }
        return res;
    }
};
```

### 分析
 - 执行用时 :80 m, 在所有 C++ 提交中击败了18.92%的用户内存消耗 :19.2 MB, 在所有 C++ 提交中击败了100.00%的用户。时间上不是很理想。可能是
   排序和循环消耗了太多时间。
 - 这道题也是一道非常简单的题。记得之前学习C的过程中，就做过一道求第k大的数。通过排序就可以很好解决。但怎么优化是一个问题。参考了解法后，看到
   网友各种各样的解法，以及对一道即使是简单的题的斟酌和细节考虑，不得不感慨需要学习的还有很多。这道题就贴上官方的solutions。但是官方的这些解
   法真的不是我这个小白能轻易想到的。即使在今天了解了其思路，但下一次真的不敢保证就能熟练运用。感觉他把一道小题做成了一个工程，非常不容易。
 - 题解中出现了一个srand()函数，查了一下，它的功能是一个随机数发生器。具体可见：https://blog.csdn.net/peixuan197/article/details/48084843
   可以和rand()函数比较学习。
 - 而且网友说直接用sort()排序面试必挂，有点小伤感。奈何我目前只会sort排序。
   
堆
```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        vector<int>vec(k, 0);
        if (k == 0) return vec; // 排除 0 的情况
        priority_queue<int>Q;
        for (int i = 0; i < k; ++i) Q.push(arr[i]);
        for (int i = k; i < (int)arr.size(); ++i) {
            if (Q.top() > arr[i]) {
                Q.pop();
                Q.push(arr[i]);
            }
        }
        for (int i = 0; i < k; ++i) {
            vec[i] = Q.top();
            Q.pop();
        }
        return vec;
    }
};

作者：LeetCode-Solution
```

快排思想
```c++
class Solution {
    int partition(vector<int>& nums, int l, int r) {
        int pivot = nums[r];
        int i = l - 1;
        for (int j = l; j <= r - 1; ++j) {
            if (nums[j] <= pivot) {
                i = i + 1;
                swap(nums[i], nums[j]);
            }
        }
        swap(nums[i + 1], nums[r]);
        return i + 1;
    }
    // 基于随机的划分
    int randomized_partition(vector<int>& nums, int l, int r) {
        int i = rand() % (r - l + 1) + l;
        swap(nums[r], nums[i]);
        return partition(nums, l, r);
    }
    void randomized_selected(vector<int>& arr, int l, int r, int k) {
        if (l >= r) return;
        int pos = randomized_partition(arr, l, r);
        int num = pos - l + 1;
        if (k == num) return;
        else if (k < num) randomized_selected(arr, l, pos - 1, k);
        else randomized_selected(arr, pos + 1, r, k - num);   
    }
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        srand((unsigned)time(NULL));
        randomized_selected(arr, 0, (int)arr.size() - 1, k);
        vector<int>vec;
        for (int i = 0; i < k; ++i) vec.push_back(arr[i]);
        return vec;
    }
};

作者：LeetCode-Solution
```
