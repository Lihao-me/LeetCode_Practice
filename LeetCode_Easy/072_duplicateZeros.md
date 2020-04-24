# 复写零
***
#### 2020.04.24

### 问题
>给你一个长度固定的整数数组 arr，请你将该数组中出现的每个零都复写一遍，并将其余的元素向右平移。                         
注意：请不要在超过该数组长度的位置写入元素。                           
要求：请对输入的数组 就地 进行上述修改，不要从函数返回任何东西。                       

### 示例
>输入：[1,0,2,3,0,4,5,0]                  
输出：null                
解释：调用函数后，输入的数组将被修改为：[1,0,0,2,3,0,0,4]                   
             
>输入：[1,2,3]            
输出：null                 
解释：调用函数后，输入的数组将被修改为：[1,2,3]                  

### 提示
1.1 <= arr.length <= 10000                  
2.0 <= arr[i] <= 9              

### 代码
```c++
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        for(int i=0;i<arr.size();i++)
        {
            if(arr[i]==0)
            {
                arr.insert(arr.begin()+i,0);
                arr.pop_back();
                i++;
            }
        }
    }
};
```

### 分析
 - 执行用时 :40 ms, 在所有 C++ 提交中击败了28.70%的用户内存消耗 :9.8 MB, 在所有 C++ 提交中击败了12.50%的用户。将0插入到0的后面，然后将最后
   一个元素弹出，这是我能想到的最好的办法了。
 - 最开始的时候想到之前的大小排序的题，考虑了一下快慢指针，但实在是做不出来啊。

原地解法
```c++
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
      
      int size = arr.size();
      int offset = 0;
      
      // 第一次遍历，计算数组中0的个数
      for (int i=0; i<size; i++) {
        if (!arr[i]) offset++;
      }
      
      // 第二次遍历，从后向前按偏移量移动元素
      for (int i=size-1; i>-1; i--) {
        if (!arr[i]) {
          offset--;
          if (i+offset < size) arr[i+offset] = 0;
          if (i+offset+1 < size) arr[i+offset+1] = 0;
        }
        else if (i+offset < size) arr[i+offset] = arr[i];
      }
    }
};

作者：5656hcx
```

逆序修改
```c++
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        if (arr.empty())
            return;
        int n = arr.size();
        int z = 0;
        for (int i : arr)
            z += i == 0;
        for (int i = n - 1; i >= 0; --i) {
            if (i + z < n)
                arr[i + z] = arr[i];
            if (arr[i] == 0) {
                z--;
                if (i + z < n)
                    arr[i + z] = 0;
            }
        }
    }
};

作者：lucifer1004
```

队列
```c++
class Solution {
public:
    void duplicateZeros(vector<int>& arr) {
        
        //队列法
        queue<int> temp;
        
        for(auto i:arr){
            
            temp.push(i);
            
            if(i==0)
                temp.push(0);
        }
        
        for(int i=0; i<arr.size(); i++){
            
            arr[i] = temp.front();
            
            temp.pop();
        }
    }
};

作者：keyway1984
