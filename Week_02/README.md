# 第二周学习工作总结
本周学习了一些新的数据结构，同时也发现了一些问题，就是C语言实现一部分的数据结构，已经显得较为吃力，比如栈和队列，遂后续很多题目都是用python来实现的。下面依旧总结一下本周的每日一题。   
***
1. 猜数字游戏（LeetCode编号：299）

你正在和你的朋友玩 猜数字（Bulls and Cows）游戏：你写下一个数字让你的朋友猜。每次他猜测后，你给他一个提示，告诉他有多少位数字和确切位置都猜对了（称为“Bulls”, 公牛），有多少位数字猜对了但是位置不对（称为“Cows”, 奶牛）。你的朋友将会根据提示继续猜，直到猜出秘密数字。请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。请注意秘密数字和朋友的猜测数都可能含有重复数字。
 
分两种情况：
+ 公牛：数字对   位置对
+ 母牛：数字对   位置不对   
当结果为公牛的时候，可直接判断，否则，用数组sum[]来记录scret中各个数字出现的次数，然后再遍历guess，找出相同的元素即可。
```
char * getHint(char * secret, char * guess){
    int i=0,j=0,A=0,B=0,sum[10]={0};
    for (i=0;secret[i];i++)
    {
        if(secret[i] == guess[i])
        {
            A++;
        } 
        else
        {
            sum[secret[i]-'0']++;
        }
    }
    for (j=0;guess[j];j++)
    {
        if(sum[guess[j]-'0'] && secret[j] != guess[j])
        {
            B++;
            sum[guess[j]-'0']--;
        }
    }
    char* res = (char*)malloc(sizeof(char)*10);
    sprintf(res, "%dA%dB", A, B);
    return res;
}
```
2. 两个数组的交集（LeetCode编号：350）  

给定两个数组，编写一个函数来计算它们的交集。    

+ 与第一个题目十分类似，还是采用哈希编码的方式，不过此次是用python编程的，所以是新建了一个字典，来记录每个数字出现的次数。
```
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        if len(nums1) > len(nums2):
            nums1,nums2 = nums2,nums1
        ex = {}
        for i in nums1:
            ex[i] = ex.get(i,0)+1
        out = []
        for j in nums2:
            if ex.get(j,0) > 0:
                out.append(j)
                ex[j] -= 1
        return out
```
31. （周三第一道题目）删除最外层的括号（LeetCode编号：1021） 

有效括号字符串为空 ("")、"(" + A + ")" 或 A + B，其中 A 和 B 都是有效的括号字符串，+ 代表字符串的连接。例如，""，"()"，"(())()" 和 "(()(()))" 都是有效的括号字符串。
如果有效字符串 S 非空，且不存在将其拆分为 S = A+B 的方法，我们称其为原语（primitive），其中 A 和 B 都是非空有效括号字符串。
给出一个非空有效字符串 S，考虑将其进行原语化分解，使得：S = P_1 + P_2 + ... + P_k，其中 P_i 是有效括号字符串原语。
对 S 进行原语化分解，删除分解中每个原语字符串的最外层括号，返回 S 。    
+ 括号的配对，很容易让人想到栈，配对则两个都弹出。
```
class Solution:
    def removeOuterParentheses(self, S: str) -> str:
        stack = []
        result = ''
        for i in S:
            if i == '(':
                stack.append(i)
                if len(stack) > 1:
                    result += '('
            else:
                stack.pop()
                if len(stack) != 0:
                    result += ')'
        return result
```
32. 滑动窗口的最大值（周三第二道题目）（LeetCode编号：面试题59 - I）链接：[面试题59 - I](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)  

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 
```
  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
+ 这个题目，第一个想到的肯定是用队列实现，可是在python中的队列需要自己创建，或者说队列是由栈实现的，因而效率很低。  
+ 后来发现了题解区有更好的答案，就是不用亲自建立列表，直接在选定的滑窗内，利用python封装好的代码max()直接找出最大值。简洁高效。

```
#解法一：
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        SlidingWindow = []
        out = []
        if len(nums) == 0:
            out = []
        else:
            for j in range (len(nums)-k+1):
                for i in range (j,j+k):
                    SlidingWindow.append(nums[i])
                    if nums[j] < nums[i]:
                        nums[j] = nums[i]
                SlidingWindow.pop(0)
                out.append(nums[j])
        return out
#解法二：
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        ans = []
        if not nums:
            return ans
        for i in range(0,len(nums)-k+1):
            ans.append(max(nums[i:i+k]))
        return ans
```
4. Fizz Buzz（LeetCode编号：412）  

写一个程序，输出从 1 到 n 数字的字符串表示。

1.如果 n 是3的倍数，输出“Fizz”；    
2.如果 n 是5的倍数，输出“Buzz”；    
3.如果 n 同时是3和5的倍数，输出 “FizzBuzz”。
+ 很简单，没有什么好说的，直接上代码。
```
class Solution:
    def fizzBuzz(self, n: int) -> List[str]:
        out  = []
        for i in range(1,n+1):
            if i%3 == 0 and i%5 == 0:
                out.append('FizzBuzz')
            elif i%3 == 0:
                out.append('Fizz')
            elif i%5 == 0:
                out.append('Buzz')
            else:
                 out.append(str(i))
        return out
```
5. 零矩阵（LeetCode编号：面试题 01.08）链接：[面试题 01.08](https://leetcode-cn.com/problems/zero-matrix-lcci/) 

编写一种算法，若M × N矩阵中某个元素为0，则将其所在的行与列清零。

示例 1：
```
输入：
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出：
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```
+ 找到矩阵的长度和宽度，建立空数组 ，用来记录0的位置，将其所在的行和列清零。
```
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        A = len(matrix)
        B = len(matrix[0])
        store = []
        for i in range(A):
            for j in range(B):
                if matrix[i][j] == 0:
                    store.append([i,j])
        for k in store:
            for i in range(A):
                matrix[i][k[1]] = 0
            for j in range(B):
                matrix[k[0]][j] = 0
```
6. 二叉树的最大深度（LeetCode编号：104）    

给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，
```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。
+ 用到了一种非常常见的代码结构———递归。
```
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root is None:
            return 0
        count1, count2 = 0, 0
        # 当前根节点的左子树是否为空
        if root.left != None:
            count1 += self.maxDepth(root.left) + 1
        # 当前根节点的右子树是否为空    
        if root.right != None:
            count2 += self.maxDepth(root.right) + 1
        # 当前节点为叶节点时，返回深度 1 
        if root.left == None and root.right == None:
            return 1
        return max(count1, count2)
```
7. 颜色分类（LeetCode编号：75） 

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。   
注意:不能使用代码库中的排序函数来解决这道题。   
示例：  
```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```
+ 基本没什么难度，直接附上代码：
```
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        store = [0,0,0]
        for i in nums:
            store[i] += 1
        nums.clear()
        if store[0] != 0:
            for j in range(store[0]):
                    nums.append(0)
        if store[1] != 0:
            for k in range(store[1]):
                    nums.append(1)
        if store[2] != 0:
            for m in range(store[2]):
                    nums.append(2)
```










   

