# 1比特与2比特字符
***
#### 2020.01.24

### 问题
>有两种特殊字符。第一种字符可以用一比特0来表示。第二种字符可以用两比特(10 或 11)来表示。
现给一个由若干比特组成的字符串。问最后一个字符是否必定为一个一比特字符。给定的字符串总是由0结束。

### 示例
>输入: 
bits = [1, 0, 0]
输出: True
解释: 
唯一的编码方式是一个两比特字符和一个一比特字符。所以最后一个字符是一比特字符。

>输入: 
bits = [1, 1, 1, 0]
输出: False
解释: 
唯一的编码方式是两比特字符和两比特字符。所以最后一个字符不是一比特字符。

>注意
1 <= len(bits) <= 1000.
bits[i] 总是0 或 1.

### 代码
```c
bool isOneBitCharacter(int* bits, int bitsSize){
    int i=0;
    if(bitsSize==1)
    return 1;
    while(1)
    {
        if(i==bitsSize-1)
        return 1;
        if(i>=bitsSize)
        return 0;
        if(bits[i]==1)
        i=i+2;
        else if(bits[i]==0)
        i=i+1;
    }

}
```

### 分析
 - 本来这事一道非常简单的题，但是却用了很长时间才做正确，起初的时候总是warning:control reaches end of non-void function的错误，
   就在于把问题考虑得太复杂把倒数第一位、第二位以及0与1的情况一一列举造成控制流返回值的遗漏。
 - 后来又出现了run time error的错误。在于循环的利用中出现纰漏。今天也是第一次接触bool型的函数，其返回值和功能上也有了更深入的了解。
 - 这一道小题可真是费了我不少时间。但当检查出错误以及有了新的简单的想法时还是很有成就感的。新的思路代码非常简洁，但时间上仅仅打败63.75%
   的人。看了不少C语言网友的题解，思路上大同小异，但一些语法的使用上则更为精炼和熟练，这是我应当积极学习的。网友的另一种解法是巧妙利用下标
   以及利用了字符串中字符数组的奇偶规律，不太好像，但贴上也算对这道题的一个补充吧。
  
  ````c
  bool isOneBitCharacter(int* bits, int bitsSize){
if(bits[bitsSize-1]==1)
    return 0;
  for(int i=2;i<=bitsSize;i++){
      if(bits[bitsSize-i]==0&&i%2==0)
          return 1;
      else if(bits[bitsSize-i]==0&&i%2==1)
          return 0;
  }
    if(bitsSize%2==0)
        return 0;
    else 
        return 1;
}
作者：xiao-gou
