# 将每个元素替换为右侧最大元素
***
#### 2020.03.01

### 问题
>给你一个数组 arr ，请你将每个元素用它右边最大的元素替换，如果是最后一个元素，用 -1 替换。
完成所有替换操作后，请你返回这个数组。

### 示例
>输入：arr = [17,18,5,4,6,1]
输出：[18,6,6,6,1,-1]

>提示：
1.1 <= arr.length <= 10^4
2.1 <= arr[i] <= 10^5

### 思路
>从左向右排查，依次找到右侧最大的数并记录。

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* replaceElements(int* arr, int arrSize, int* returnSize){
    int *a;
    a=(int*)malloc(arrSize*sizeof(int));
    *returnSize=arrSize;
    for(int i=0;i<arrSize;i++){
        if(i==arrSize-1){
        a[i]=-1;break;}
        int max=0;
        for(int j=i+1;j<arrSize;j++){
            if(arr[j]>max)
            max=arr[j];
        }
        a[i]=max;
    }
    return a;

}
```
优化内存后
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* replaceElements(int* arr, int arrSize, int* returnSize){
    *returnSize=arrSize;
    for(int i=0;i<arrSize;i++){
        if(i==arrSize-1){
        arr[i]=-1;break;}
        int max=0;
        for(int j=i+1;j<arrSize;j++){
            if(arr[j]>max)
            max=arr[j];
        }
        arr[i]=max;
    }
    return arr;

}
```

### 分析
 - 由于题目并没有要求返回原数组名，所以重新申请一个数组大大消耗内存，优化后直接在原数组上修改。第一次提交：执行用时：728 ms战胜 13.33 % 的 c 提交
   记录，内存消耗：13.9 MB战胜 26.76 % 的 c 提交记录。修改后执行用时 :568 ms, 在所有 C 提交中击败了30.83%的用户内存消耗 :13.6 MB, 在所有 C 提
   交中击败了80.28%的用户。
 - 事实上我的算法是正序，而逆序还能优化时间。
 
设输入N个数据。从右边起构建数组，时间复杂度为O(N);从左边起构建数组，时间复杂度为O(N^2)。
```c
int* replaceElements(int* arr, int arrSize, int* returnSize){
    * returnSize=arrSize;
    int *res=malloc(arrSize*sizeof(int));
    int i,max=arr[arrSize-1];
    res[arrSize-1]=-1;
    for(i=arrSize-2;i>=0;i--){
        res[i]=max;
        if(arr[i]>max) max=arr[i];
    }
    return res;
}

作者：bevischou
