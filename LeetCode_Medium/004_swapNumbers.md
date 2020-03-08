# 交换数字
***
#### 2020.03.08

### 问题
>编写一个函数，不用临时变量，直接交换numbers = [a, b]中a与b的值。

### 示例
>输入: numbers = [1,2]   
输出: [2,1]   

>提示：
numbers.length == 2

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* swapNumbers(int* numbers, int numbersSize, int* returnSize){
    *returnSize=numbersSize;
    numbers[0]=numbers[0]^numbers[1];
    numbers[1]=numbers[1]^numbers[0];
    numbers[0]=numbers[0]^numbers[1];
    return numbers;

}
```

### 分析
 - 执行用时 :0 ms, 在所有 C 提交中击败了100.00%的用户内存消耗 :6.8 MB, 在所有 C 提交中击败了100.00%的用户。虽然是一道非常简单的题，但是迎来了人生
   的第一个双百，还是非常高兴的。
 - 如果是以前，可能更多想到的是新申请一个数组或者用函数的方法来解决。这是今天刚学习的位运算的方法，结果正好可以在这里得到运用。关于位运算的几个符号
   可以详见：
   https://blog.csdn.net/qq_41722217/article/details/89609467 。“^”通过二进制不相同取1，否则为0的运算法则实现不通过临时变量交换两数，而且由于
   是二进制的运算，大大缩短了时间。而只是在原数组中的元素修改，也没有消耗太多内存。
 - 网友提供的求和相加法也是我没有想到过的，也值得学习。

```c++
class Solution {
    // 求和相减法
    public int[] swapNumbers(int[] numbers) {
        numbers[0] += numbers[1];
        numbers[1] = numbers[0] - numbers[1];
        numbers[0] -= numbers[1];
        return numbers;
    }
};
作者：sun-xiao-chuan-2-5-8
