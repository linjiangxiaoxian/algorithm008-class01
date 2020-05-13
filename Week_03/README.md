# 第三周学习工作总结
本周部分题目明显偏难，比如求公共祖先，求岛屿数据，个人以为，对于DFS和BFS问题的熟练度明显不够，需要在这方面进行加强。关于程序本身，感觉对于递归的认识不够，还需要进一步加强。
***
1. 两数之和（LeetCode编号：1）

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。    

建立字典，记录每个数字及其出现的位置，若与另外一个数字之和为targer，并且顺序不相等，则返回这两个数。    

```
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dic ={}
        for ind1,num in enumerate(nums):
            dic[num] = ind1
        for ind2,num in enumerate(nums):
            ind3 = dic.get(target-num)
            if ind3 != ind2 and ind3 != None:
                return (ind2,ind3)
```
21. 字符串相加（LeetCode编号：415）  


给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

注意：

num1 和num2 的长度都小于 5100.
num1 和num2 都只包含数字 0-9.
num1 和num2 都不包含任何前导零。
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。 

这道题看似简单，其实需要考虑很多种情况：
+ 没有低位的进位，本位没有溢出
+ 没有低位的进位，本位溢出
+ 有低位的进位，本位没有溢出
+ 有低位的进位，本位溢出
```
'''
其中循环内的add是普通的进位，而return中的进位是当前最高位向上面一位的进位。
'''
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        res = ""
        i,j,add = len(num1)-1,len(num2)-1,0
        while(i >=0 or j >=0):
            a = int(num1[i]) if i >= 0 else 0
            b = int(num2[j]) if j >= 0 else 0
            if add:
                res = str((a + b + 1)%10) + res 
            else:
                res = str((a + b)%10) + res
            add = (a + b + add) // 10
            i,j = i-1,j-1
        return "1" + res if add else res
```
22. 滑动窗口的最大值（LeetCode编号：239）    
给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。    
示例： 
 ```
 输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 ```
 这道题目非常简单，我以为考察的是队列，其实也可以从另外一个方面想，不需要将滑窗的值专门存放在一个队列或者是栈中，只需要每次逐个访问窗口中的值即可。 
 我的解法：
 ```
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
 ```
 别人的解法：
 ```
 class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        ans = []
        if not nums:
            return ans
        for i in range(0,len(nums)-k+1):
            ans.append(max(nums[i:i+k]))
        return ans
 ```
 3. 替换空格（LeetCode编号：面试题05）  

 请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

示例 1：    
```
输入：s = "We are happy."
输出："We%20are%20happy."
```
没什么好说的，这个简单  
```
class Solution:
    def replaceSpace(self, s: str) -> str:
        listx = []
        for i in s:
            if i == ' ':
                listx.append('%20')
            else:
                listx.append(i)
        return "".join(listx)
``` 
4. 二叉树的最大深度（LeetCode编号：104）    

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
返回它的最大深度3。 

这道题目是关于二叉树的访问问题，比较基础，这样 的题目应该多做，多熟悉。 
```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

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
5. 岛屿数量（LeetCode编号：200） 
   
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。
示例 1:     
```
输入:
11110
11010
11000
00000
输出: 1
```
示例 2 ：
```
输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平和/或竖直方向上相邻的陆地连接而成。
```
这道题常见的解法是DFS,还是比较难的题目。
```
#发现陆地
#结果加一
#访问清零
#陆地扩张
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        res = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == "1":
                    res += 1
                    grid[i][j] = "0"
                    groung_position = []
                    groung_position.append([i,j])
                    while len(groung_position) > 0:
                        x,y = groung_position.pop()
                        for new_x,new_y in [[x,y+1],[x,y-1],[x-1,y],[x+1,y]]:
                            if 0 <= new_x <len(grid) and 0 <= new_y <len(grid[0]) and grid[new_x][new_y] =="1":
                                groung_position.append([new_x,new_y])
                                grid[new_x][new_y] = "0"
        return res
```
6. 三数之和（LeetCode编号：15）
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。

注意：答案中不可以包含重复的三元组。

示例：
```
给定数组 nums = [-1, 0, 1, 2, -1, -4]，

满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```
分析：固定k，然后i，j作为双指针，，逐渐逼近所要找的值即可。
```
#创建res作为最终的结果，k,i,j作为a,b,c在数组中的位置
'''class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:'''
class Solution:
    def threeSum(self, nums: [int]) -> [[int]]:
        nums.sort()
        res,k = [],0
        for k in range(len(nums)-2):
            if nums[k] > 0:
                break
            if k >0 and nums[k] == nums[k-1]:
                continue
            i,j = k+1,len(nums)-1
            while i < j:
                add = nums[k] + nums[i] + nums[j]
                if add < 0:
                    i += 1
                    while i < j and nums[i] == nums[i-1]:
                        i += 1
                elif add > 0:
                    j -= 1
                    while i < j and nums[j] == nums[j+1]:
                        j -= 1
                else:
                    res.append([nums[k],nums[i],nums[j]])
                    i += 1
                    j -= 1
                    while i < j and nums[i] == nums[i-1]:
                        i += 1
                    while i <j and nums[j] == nums[j+1]:
                        j -= 1
        return res
```
7. 有效的字母异位词（LeetCode编号：242）    
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1:
```
输入: s = "anagram", t = "nagaram"
输出: true
```
这个题目可以用暴力求解，可是求解的时间复杂度相当高。    
解法一：暴力求解    
```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        ex1 = {}
        ex2 = {}
        m,n = 0,0
        for i in s:
            ex1[i] = ex1.get(i,0)+1
        for j in t:
            ex2[j] = ex2.get(j,0)+1
        for i in ex1:
            if i in ex2 and ex1[i] == ex2[i]:
                m = 0
            else:
                m = 1
                break
        for j in ex2:
            if j in ex1 and ex1[j] == ex2[j]:
                n = 0
            else:
                n = 1
                break
        if not m and not n:
            return True
        else:
            return False
```
解法二：简单快捷   就一行解决问题  用时最短 
先排序，再比较
```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:    
        return True if sorted(s)==sorted(t) else False
```
解法三：分别用两个字典记录每个数字出现的次数，然后再比价其异同  
先排序，再比较
```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        ex1 = {}
        ex2 = {}

        for i in s:
            ex1[i] = ex1.get(i,0) + 1
        for j in t:
            ex2[j] = ex2.get(j,0) + 1
        if ex1 == ex2:
            return True
        else:
            return False
```









