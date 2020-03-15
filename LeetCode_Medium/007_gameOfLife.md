# 生命游戏
***
#### 2020.03.15

### 问题
>根据百度百科，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在1970年发明的细胞自动机。
给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞具有一个初始状态 live（1）即为活细胞，
或 dead（0）即为死细胞。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：    
1.如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；     
2.如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；     
3.如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；    
4.如果死细胞周围正好有三个活细胞，则该位置死细胞复活；    
根据当前状态，写一个函数来计算面板上细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态
下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

### 示例
>输入:      
[       
  [0,1,0],      
  [0,0,1],     
  [1,1,1],      
  [0,0,0]        
]        

>输出:       
[       
  [0,0,0],      
  [1,0,1],      
  [0,1,1],      
  [0,1,0]      
]       

### 进阶
>进阶:
	1.你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。   
	2.本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？    

### 思路
>一次遍历整个数组。如果元素值大于零且符合生存条件则值不变，若不符合则计为2；若元素值小于1且符合复活条件，则计为-1。最后将所有的2转化为0，将所有的
-1转化为1。

### 代码
```c
void gameOfLife(int** board, int boardSize, int* boardColSize){
    for(int i=0;i<boardSize;i++){
        for(int j=0;j<*boardColSize;j++){
            int num=0;
            if(i-1>=0&&j-1>=0&&board[i-1][j-1]>0)   num++;
            if(i-1>=0&&board[i-1][j]>0)    num++;
            if(i-1>=0&&j+1<=*boardColSize-1&&board[i-1][j+1]>0)    num++;
            if(j-1>=0&&board[i][j-1]>0)    num++;
            if(j+1<=*boardColSize-1&&board[i][j+1]>0)    num++;
            if(i+1<=boardSize-1&&j-1>=0&&board[i+1][j-1]>0)   num++;
            if(i+1<=boardSize-1&&board[i+1][j]>0)     num++;
            if(i+1<=boardSize-1&&j+1<=*boardColSize-1&&board[i+1][j+1]>0)  num++;
            if(board[i][j]>0){
                if(num<2||num>3)
                board[i][j]=2;//2 is the symble of going to die but alive now
            }
            else{
                if(num==3)
                board[i][j]=-1;//-1 is the symble of gong to be alive but dying now
            }
          }
    }
    for(int i=0;i<boardSize;i++){
        for(int j=0;j<*boardColSize;j++){
            if(board[i][j]==2){
                board[i][j]=0;
            }
            if(board[i][j]==-1){
                board[i][j]=1;
            }
            
        }
    }
    return *board;
}
```

### 分析
 - 这道题的一个难点在于细胞的死与活是同时进行的。看到这道题的第一想法是申请一个字符二维数组，用这个数组来储存细胞信息。但是看到进阶的提示后可以想见
   是完全可以基于原数组进行操作的。这样就可以通过信息的数字化处理进行解决。
 - 执行用时 :0 ms, 在所有 C 提交中击败了100.00%的用户内存消耗 :5.6 MB, 在所有 C 提交中击败了100.00%的用户。又是一个双百，但明明很暴力的解法我却不
   知道它的优势在哪里。网友的位运算值得一看。
   
```c++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        if (board.empty())
            return;
        int R = board.size();
        int C = board[0].size();
        int nearby[8][2] = {{-1, 0}, {-1, -1}, {0, -1}, {1, -1}, {1, 0}, {1, 1}, {0, 1}, {-1, 1}};
        for (int i = 0; i < R; ++i) {
            for (int j = 0; j < C; ++j) {
                for (int k = 0; k < 8; ++k) {
                    int r = i + nearby[k][0];
                    int c = j + nearby[k][1];
                    if (r >= 0 && r < R && c >= 0 && c < C) {
                        board[i][j] += (board[r][c] & 1) << 1;
                    }
                }
            }
        }
        for (int i = 0; i < R; ++i) {
            for (int j = 0; j < C; ++j) {
                int t = board[i][j] >> 1;
                if (t < 2 || t > 3) {
                    board[i][j] = 0;
                } else if (t == 3) {
                    board[i][j] = 1;
                } else {
                    board[i][j] &= 1;
                }
            }
        }
        return;
    }
};

作者：da-li-wang
