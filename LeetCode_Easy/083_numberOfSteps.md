# 将数字变成0的操作次数
***
#### 2020.05.10

### 问题
>给你一个非负整数 num ，请你返回将它变成 0 所需要的步数。 如果当前数字是偶数，你需要把它除以 2 ；否则，减去 1 。                           

### 示例
>输入：num = 14               
输出：6              
解释：                                   
步骤 1) 14 是偶数，除以 2 得到 7 。                      
步骤 2） 7 是奇数，减 1 得到 6 。                               
步骤 3） 6 是偶数，除以 2 得到 3 。                         
步骤 4） 3 是奇数，减 1 得到 2 。                  
步骤 5） 2 是偶数，除以 2 得到 1 。                   
步骤 6） 1 是奇数，减 1 得到 0 。                                                

>输入：num = 8                       
输出：4                
解释：                 
步骤 1） 8 是偶数，除以 2 得到 4 。                            
步骤 2） 4 是偶数，除以 2 得到 2 。              
步骤 3） 2 是偶数，除以 2 得到 1 。                 
步骤 4） 1 是奇数，减 1 得到 0 。                              

>输入：num = 123                   
输出：12               

### 提示
>0 <= num <= 10^6        

### 代码
```c++
class Solution {
public:
    int count=0;
    int deal(int n)
    {
        if(n==0)
        return 0;
        else if(n%2==0)
        {
            n=n/2;
            count++;
            return deal(n);
        }
        else if(n%2!=0)
        {
            n=n-1;
            count++;
            return deal(n);
        }
        return 0;
    }
    int numberOfSteps (int num) {
        deal(num);
        return count;
    }
};
```

### 分析
 - 执行用时 :4 ms, 在所有 C++ 提交中击败了51.80%的用户内存消耗 :6.1 MB, 在所有 C++ 提交中击败了100.00%的用户。首先想到的还是递归的思路，用
   递归完成操作，并记录运算的步数。但是从时间的击败人数上来看，还有更好的解决办法。看到题解中也有即使是递归但比我更好的递归方式。
   
三目运算法
```c++
class Solution {
public:
    int numberOfSteps (int num) {
        int k = 0;
        while(num > 0){
            num%2==0?num/=2:num-=1;
            k++;
        }
        return k;
    }
};

作者：jue-geng-zhe
```

右移
```c++
class Solution {
public:
    int numberOfSteps (int num) {
          if(num==0) return num;
          int count=0;
          while(num)
          {
             if(num&1) count+=2;
             else count++;
             num>>=1;
          }
          count--;
          return count;
    }
};

作者：chuxuan-2
```

递归
```c++
class Solution {
public:
    int numberOfSteps (int num) {
        if(num==1) return 1;
        if(num%2==0) return numberOfSteps(num/2)+1;
        return numberOfSteps(num-1)+1;
    }
};

作者：adonis-7
