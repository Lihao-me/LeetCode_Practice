# 加一
***
#### 2020.2.1

### 问题
>给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。
最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。
你可以假设除了整数 0 之外，这个整数不会以零开头。

### 示例
>输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。

>输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。

### 思路
>如果数组最后一项小于9，则加一；如果数组最后一项为9，则这一项变为0其前一项加一，若前一项也是9则重复此操作；特殊情况下一直加到第一项也为9则第一项变为
0，其前增加一项且为1。

### 代码
```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* plusOne(int* digits, int digitsSize, int* returnSize){
    int i=digitsSize-1;
    if(digits[digitsSize-1]<9){
    digits[digitsSize-1]=digits[digitsSize-1]+1;
    * returnSize=digitsSize;
    return digits;}
    else if(digits[digitsSize-1]==9)
    {
        while(i>=0){
            if(i>=1&&digits[i]==9)
            {
                digits[i]=0;
                i=i-1;
                continue;
            }
            else if(digits[i]<9){
            digits[i]=digits[i]+1;
            * returnSize=digitsSize;
            return digits;}
            else if(i==0&&digits[i]==9){
                int *add=(int *)malloc((digitsSize+1)*sizeof(int));
                * returnSize=digitsSize+1;            
                digits[0]==0;
                
                for(int j=1;j<=digitsSize;j++){
                add[j]=0;}
                add[0]=1;
                return add;
            }
        }
    }
return 0;
}
```

### 分析
 - 这道题的思路还是非常清晰的，但是代码的实现还是费了一些力气。
 - 好久没有写这种分支稍微多一点的代码有点生疏了，很长的时间在思考剩下的基本都在修改编译错误。比如申请数组，条件考虑不到位，漏掉返回值。ps:LeetCode
   上有一种错误很值得注意error:control reaches end of non-void function[-Werror=return-type]代码也没问题但就是报这个错误，网友的解答是：
   在函数的最后添加一条return语句，只要返回的数据类型对即可！记住，一定要返回，虽然永远也用不上。对于这个错误的字面理解我认为应该是出于函数的完整性
   的考虑。
 - 耗时击败78.79%的人也还好，但内存仅击败21.61%的C用户，代码确实有点冗长了，欣赏了网友相同思路下的代码确实发现自己的代码有许多可以合并简化的地方。
   思路上C应该也没有更简洁的思路了，那就贴上一个代码更为简洁的C答案以供参考学习。
   
   ```c
   int* plusOne(int* digits, int digitsSize, int* returnSize){
    int *num = (int*)malloc(sizeof(int) * (digitsSize + 1));
    num[0] = 1;
    int i;
    for (i = digitsSize - 1; i >= 0; i--){
        if (digits[i] == 9)
            digits[i] = 0;
        else{
            digits[i] += 1;
            break;
        }
    }
    *returnSize = (digits[0] == 0) ? (digitsSize + 1) : digitsSize;
    for (i = 0; i < digitsSize; i++){
        num[i + 1] = digits[i];
    }
    return (digits[0] == 0) ? num : digits;       
   }
   作者：wpy1010011010
