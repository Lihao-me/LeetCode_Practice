# x的平方根
***
#### 2020.05.18

### 问题
>实现 int sqrt(int x) 函数。          
计算并返回 x 的平方根，其中 x 是非负整数。                                      
由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。                                                                   

### 示例
>输入: 4                         
输出: 2                                        

>输入: 8            
输出: 2              

### 说明
>8 的平方根是 2.82842..., 
由于返回类型是整数，小数部分将被舍去。

### 代码
```c++
class Solution {
public:
    int mySqrt(int x) {
        //return (int)sqrt(x);

        for(int i=1;i<=x;i++)
        {
            if(x-i*i<2*i+1)
            return i;
        }
        return 0;
    }
};
```

### 分析
 - 执行用时 :40 ms, 在所有 C++ 提交中击败了6.51%的用户内存消耗 :5.9 MB, 在所有 C++ 提交中击败了100.00%的用户。第一次采用暴力解法时发生
   溢出。主要是在判断(i+1)*(i+1)时。所以改进了一下。当然最好的解法还是二分法。
   
### 优解
注意：1.如果用for循环一个个去遍历，肯定会超时，所以考虑二分法
2.二分法判断的核心:x/mid==mid||(x/mid>mid&&x/(mid+1)<(mid+1))当mid平方刚好=x，当然OK；另外，如果mid平方<x,mid+1的平方>x，也OK
3.注意不要用mid*mid这样，会造成整型溢出，可以用x/mid>=<mid来进行判断
```c++
class Solution {//二分法呀,但是不能用平方，会溢出，用除
public:
    int mySqrt(int x) {
        if(x==1||x==0)return x;
        int left=0,right=x/2+1;
        while(left<right){
            int mid=left+(right-left)/2;
            if(x/mid==mid||(x/mid>mid&&x/(mid+1)<(mid+1))){//刚好
                return mid;
            }
            else if(x/mid>mid){//mid比根小
                left=mid;
            }
            else{//mid比根大
                right=mid;
            }
        }
        return -1;
    }
};

作者：from_zero-2
