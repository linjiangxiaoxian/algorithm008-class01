# 第一周学习工作总结
本周是算法训练营的开营之周，加之此前基础薄弱，开始便是困难重重，如履薄冰，单单Git的安装就耗费了较多的时间，接踵而至的，是课程各种新概念和GitHub等等操作的水土不服。 
此前仅仅是本科用C语言进行嵌入式开发，而后研究生阶段用python进行人工智能方面的开发和研究。两个语言都学得不是很深，浅尝辄止。也许关于知识点本身，我的认识不会太到位，仅仅总结一些自己在代码方面的觉得精彩的地方吧。
***
经典代码总结：
1. 移动零（LeetCode编号：283）

python版解法：  
```
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        j = 0
        for i in nums:
            if i == 0:
                pass
            else:
                nums[j] = i
                j += 1
        for i in range(j,len(nums)):
            nums[i] = 0
```  

方法一：这是入门级的题目，开始的时候没有什么经验，就采用的最为暴力的方法。  
```
void moveZeroes(int* nums, int numsSize){
    int i=0,j=0,k=0;
for(j=0;j<numsSize-1;j++)
    for(i=0;i<numsSize;i++)
    {
        if (nums[i] == 0 && i < numsSize-1)
        {
            k = nums[i+1];
            nums[i+1] = nums[i];
            nums[i] = k;
        } 
  
    }
}
```
方法二：后来看到一种比较简洁的写法，只需要遍历一遍，先将数组不为零的数字全都移动到前方，然后后面的再添加零即可。
```
void moveZeroes(int* nums, int numsSize){
    int i=0,j=0,k=0;
    for(i=0;i<numsSize;i++)
    {
        if (nums[i] != 0)
            {
                nums[j] = nums[i];
                if (i != j)
                {
                    nums[i] = 0;
                }
                
                j++;
            }
    }
}
```
2. 加一（LeetCode编号：66），这个问题看着简单，最难的地方就在于进位，需要分不同的情况进行讨论。
+ 全是9；
+ 末尾是9，中间某些位置是9；
+ 末尾没有9，也就是所谓的一般情况，这个时候没有进位，只需要在末尾以为+1即可。
```
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* plusOne(int* digits, int digitsSize, int* returnSize){
    int i=0,j=0,k=0,m=0;
    for (i = 0;i<digitsSize;i++)
    {
        if (digits[i] == 9)
        {
            j++;
        }
    }
    if (j == digitsSize)
    {
        int * a = (int * )malloc(sizeof(int)*(digitsSize+1));
        a[0] = 1;
        for (k=1;k<=digitsSize;k++)
        {
            a[k] = 0;
        }
        *returnSize = digitsSize+1;
        return a;
    }
    else if (j>0 && digits[digitsSize-1] == 9)
    {
        for(m=digitsSize-1;digits[m] == 9;m--)
        {
            digits[m] = 0;
        }
        digits[m]++;
        *returnSize = digitsSize;
        return digits;
    }
    else
    {
        digits[digitsSize - 1] ++;
        *returnSize = digitsSize;
        return digits;
    }
}

```
3. 旋转数组（LeetCode编号：189）
给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。    
这个题目看似简单，实际这个数组是一个旋转的结构，如何构造旋转数组便变得尤为重要。又可以理解为拆解旋转结构。
```
void rotate(int* nums, int numsSize, int k){
    int i=0,j=0,m=0,n=0,nums2[2*numsSize];
    k = k % numsSize;
    for (i=0;i<numsSize;i++)
    {
       if (i == numsSize - k && numsSize > 1)
       {
           for (j=i;j<numsSize;j++)
           {
               nums2[j] = nums[j];
           }
           for (m=numsSize-1;m>=k;m--)
           {
               nums[m] = nums[m-k];
           }
           for (n=0;n<k;n++)
           {
               nums[n] = nums2[n+i];
           }
       } 
    }  
}
```
4. 整数的各位积和之差（LeetCode编号：1281） 

给你一个整数 n，请你帮忙计算并返回该整数「各位数字之积」与「各位数字之和」的差。

需要特别讨论的，就是整数只有一位的情况，这个时候就不能按照原始的公式进行计算。   
```
int subtractProductAndSum(int n){
    
    int i,j,mul=1,add=0;
    if (n == 1)
    {
       mul = n;
       add = n;
    }
    else
    {
        for (i=1; i<n; i = i*10)
        {
            j = n / i;
            j = j % 10;
            mul = mul * j;
            add = add + j;
        }
    }
    return mul - add;
}
``` 
5. 有序数组的平方（LeetCode编号：977)   
给定一个按非递减顺序排序的整数数组 A，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。  
+ 如果不使用双指针，将会非常浪费时间。
+ 先找出正负数的分界点，然后再使用双指针依次向两边移动，来比较大小，放入新的数组。
```
int* sortedSquares(int* a, int n, int* returnSize){
	*returnSize=n;
    int i=0,j=0,k=0,left=-1,right=0;
    if (n == 0)
    {
        return a;
    }
    for (i=0;i<n;i++)
    {
        if (a[i]>=0)
        {
            left = i-1;
            right = i;
            break;
        }
    }
	int *res=(int *)malloc(sizeof(int)*n);
    while(left>=0 && right<n)
    {
        if(a[left]*a[left] < a[right]*a[right])
        {
            res[k++] = a[left]*a[left];
            left--;
        }
        else
        {
            res[k++] = a[right]*a[right];
            right++;
        }
    }
    while(right<n)
    {
        res[k++] = a[right]*a[right];
        right++;
    }
    while(left>=0)
    {
        res[k++] = a[left]*a[left];
        left--;
    }
return res;
}
```
6. 仅仅反转字母（LeetCode编号：917）
给定一个字符串 S，返回 “反转后的” 字符串，其中不是字母的字符都保留在原地，而所有字母的位置发生反转。
+ 一开始的时候根本一头雾水，因为主要的难点在于如何识别大小写字母
+ 难点二在于如何保持其他部分不变
+ 同样是利用双指针来解决问题，这次是从两边往中间靠。
```
char * reverseOnlyLetters(char * S){
    int length=0,left=0,right=0;
    while(S[length]!=0) length++;
    char ex[length+1];
    ex[length] = 0;
    right = length-1;
    while(left<=right)
    {
        if (!(S[left]>='A' && S[left]<='Z' || S[left]>='a' && S[left]<='z'))
        {
            ex[left] = S[left];
            left++;
            continue;
        }
        if (!(S[right]>='A' && S[right]<='Z' || S[right]>='a' && S[right]<='z'))
        {
            ex[right] = S[right];
            right--;
            continue;
        }
        ex[left] = S[right];
        ex[right] = S[left];
        right--;
        left++;
    }
    S = ex;
    return S;
}

```
7. 第一个只出现一次的字符   
在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 
+ 难点在于如何统计字符串出现的次数，因为这方面没有任何的经验。
```
char firstUniqChar(char* s){
    int i,j,cnt[256]={0}; 
    for (i=0;s[i]!='\0';i++)
    {
        cnt[s[i]]++;
    }
    for (j=0;s[j]!='\0';j++)
    {
        if (cnt[s[j]]==1)
        return s[j];
    }
    return ' ';
}
```


