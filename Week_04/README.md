# 第四周学习工作总结    
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree  
本周任务相当繁重，很多题目感觉无从下手，而且非常难，比如扫雷问题。  
本周题目概览：  
```
星期一：day22: lc70-爬楼梯
星期二：day23: lc433最小基因变化
星期三：day24: lc529扫雷游戏
星期四：day25: lc169多数元素 
星期五：day26: lc面试题49.丑数
星期六：day27: lc51N皇后
星期天：day28: lc256.二叉树的最近公共祖先
```  
1. 爬楼梯   

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：
```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```
分析：这道题目已经很熟悉了，解题思路就是找重复的操作，也就是爬楼梯时的重复操作，这里有两种解法，大同小异。  
```
#解法1:用时36ms
class Solution:
    def climbStairs(self, n: int) -> int:
        f1,f2,f3 = 1,2,0
        for i in range(2,n):
            f3 = f2 + f1
            f1 = f2
            f2 = f3
        return max(f3,n)
```
```
#解法2：用时40ms
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <=2:
           return n
        f1,f2,f3 = 1,2,3
        for i in range(3,n+1):
            f3 = f2 + f1
            f1 = f2
            f2 = f3
        return f3
```
2. 最小基因变化     

一条基因序列由一个带有8个字符的字符串表示，其中每个字符都属于 "A", "C", "G", "T"中的任意一个。

假设我们要调查一个基因序列的变化。一次基因变化意味着这个基因序列中的一个字符发生了变化。

例如，基因序列由"AACCGGTT" 变化至 "AACCGGTA" 即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — start, end, bank，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。

注意:

+ 起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
+ 所有的目标基因序列必须是合法的。
+ 假定起始基因序列与目标基因序列是不一样的。    
示例1：
```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

返回值: 1
```
示例2：
```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

返回值: 2
```
示例3：
```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

返回值: 3
``` 
这道题目可以理解为一个跳板，先从start->bank->end有一点很重要，就是从start到bank只需要一步，而从bank到end需要若干步。
代码如下：  
```
class Solution:
    def minMutation(self, start: str, end: str, bank: List[str]) -> int:
        if start == end: return 0
        if not bank: return -1
        if end not in bank: return -1
        def get_diffirence(s1: str, s2:str) -> int:
            return sum([1 for i in range(8) if s1[i]!=s2[i]])
        dif_list1 = [get_diffirence(start, _) for _ in bank]
        dif_list2 = [-1 for _ in bank]
        from collections import deque
        cur, result = deque([bank.index(end)]), float('inf')
        dif_list2[bank.index(end)] = 0
        while cur: # BFS
            temp_index = cur.pop()
            for i, _ in enumerate(bank):
                if dif_list2[i] > -1: continue
                if get_diffirence(_, bank[temp_index]) == 1:
                    cur.appendleft(i)
                    dif_list2[i] = dif_list2[temp_index] + 1
        for i, _ in enumerate(dif_list1):
            if _ <= 1 and dif_list2[i]>-1:
                result = min(result, _ + dif_list2[i])
        return result if result<float('inf') else -1
``` 
3.  扫雷游戏    

让我们一起来玩扫雷游戏！

给定一个代表游戏板的二维字符矩阵。 'M' 代表一个未挖出的地雷，'E' 代表一个未挖出的空方块，'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的已挖出的空白方块，数字（'1' 到 '8'）表示有多少地雷与这块已挖出的方块相邻，'X' 则表示一个已挖出的地雷。

现在给出在所有未挖出的方块中（'M'或者'E'）的下一个点击位置（行和列索引），根据以下规则，返回相应位置被点击后对应的面板：

如果一个地雷（'M'）被挖出，游戏就结束了- 把它改为 'X'。
如果一个没有相邻地雷的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的方块都应该被递归地揭露。
如果一个至少与一个地雷相邻的空方块（'E'）被挖出，修改它为数字（'1'到'8'），表示相邻地雷的数量。
如果在此次点击中，若无更多方块可被揭露，则返回面板。    
示例1：    
```
输入: 

[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]

Click : [3,0]

输出: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
``` 
示例2：
```
输入: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Click : [1,2]

输出: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]
``` 
分析：这是一道DFS的问题，而且有一点非常难，就是区域的终止条件，应该为B为中心的周围的8个点，若中心点不为B,则停止搜索。   
```
#DFS首先设定边界条件如果不是‘E’return。剩下的就是和200岛屿问题一样进行DFS，只不过这里是八连通
class Solution:
    def updateBoard(self, board: List[List[str]], click: List[int]) -> List[List[str]]:
        if not board:
            return []
        i,j = click[0],click[1]
        if board[i][j] == 'M':
            board[i][j] = 'X'
            return board
        self.dfs(board,i,j)
        return board
    def dfs(self,board,i,j):
        m,n = len(board),len(board[0])
        if board[i][j] != 'E':
            return
        directions = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]
        add = 0
        for d in directions:
            new_i,new_j = i+d[0],j+d[1]
            if 0<=new_i<m and 0<=new_j<n and board[new_i][new_j] == 'M':
                add += 1
        if add == 0:
            board[i][j] = 'B'
        else:
            board[i][j] = str(add)
            return
        for d in directions:
            new_i,new_j = i+d[0],j+d[1]
            if 0<=new_i<m and 0<=new_j<n:
                self.dfs(board,new_i,new_j)
```
4. 多数元素     

给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于 ⌊ n/2 ⌋ 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。    
分析：这道题目非常简单，可以自己编出来，用一个字典，记录每个元素出现的次数。
  ```
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        more_element = {}
        for i in nums:
            more_element[i] = more_element.get(i,0) + 1
        for key,value in more_element.items():
            if value > len(nums) // 2:
                return key
                break
```
5. 丑数 
我们把只包含因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

分析：这道题目看似简单，实则非常复杂，因为只包含因子，而不是包含因子，所以是个动态规划的问题。

```
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        dp,a,b,c = [1]*n,0,0,0
        for i in range(1,n):
            n2 = dp[a] * 2
            n3 = dp[b] * 3
            n5 = dp[c] * 5
            dp[i] = min(n2,n3,n5)
            if dp[i] == n2: a += 1
            if dp[i] == n3: b += 1
            if dp[i] == n5: c += 1
        return dp[-1]
``` 
6. N皇后    
n 皇后问题研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。  
官方显示这道题目为困难的级别，也确实比较难，最难的地方在于如何找到所有可能的结果，我觉得这是比较难的地方，也就是一种回溯的思想。    
```
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        ans = list()
        res = [['.']*n for i in range(n)] # 保存最终的结果
        '''三个方向'''
        col = [False] *2*n # 指示某一列是否可以放元素
        dg = [False] *2*n # 指示对角线是否合法
        udg = [False] *2*n # 指示反对角线是否合法

        def dfs(u):
            list_1 = list()
            # 搜到一个可行答案，皇后数=行数=列数
            if u == n:
                for i in range(n):
                    list_1.append(''.join(res[i]))
                ans.append(list_1)
                return
            
            # 判断应该在哪一列放皇后
            for i in range(n):
                if not col[i] and not dg[u+i] and not udg[n-u+i]:
                    
                    # 放置皇后
                    res[u][i] = 'Q'
                    col[i] = dg[u+i] = udg[n-u+i] = True
                    dfs(u+1)
                    # 恢复现场
                    res[u][i] = '.'
                    col[i] = dg[u+i] = udg[n-u+i] = False
        
        # 从第0行开始搜
        dfs(0)
        return ans
```
7.  二叉树的最近公共祖先    

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
```
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。  
``` 
分析：分成三种情况：
+ p 和 qq 在 rootroot 的子树中，且分列 rootroot 的 异侧（即分别在左、右子树中）
+ p=root ，且 q 在 root 的左或右子树中
+ q=root ，且 p 在 root 的左或右子树中
 
然后对这三种情况分而治之即可。  
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if not root or root == p or root == q:
            return root
        left = self.lowestCommonAncestor(root.left,p,q)
        right = self.lowestCommonAncestor(root.right,p,q)
        if not left:
            return right
        if not right:
            return left
        return root
```


