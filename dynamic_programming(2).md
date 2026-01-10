# 26/1/10
## Given two strings s1 and s2, return the lowest ASCII sum of deleted characters to make two strings equal.

Example 1:

Input: s1 = "sea", s2 = "eat"
Output: 231
Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
Deleting "t" from "eat" adds 116 to the sum.
At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.

給定兩個字串 as1和 s2b，在傳回刪除字元後使兩個字串相等的最小ASCII和。

例1：

輸入： s1 = "sea", s2 = "eat"
輸出： 231
說明：從 "sea" 中刪除 "s" 會將 "s" 的 ASCII 值 (115) 加到總和中。
從“eat”中刪除“t”會使總和增加116。
最後，兩個字串相等，115 + 116 = 231 是實現此目的的最小和。

### 這題跟1/9一樣 重點在於如何繪製表格 一開始想到要把兩個字串做對比 一樣的就填入1，否則填入0 但是發現無法處理下一列時在 j<current_j 遇到1時該如何處理 後來的解決方法為把一樣的char換成ascii填入表格

![da139b79-b664-4415-b39b-22e71f78d813](https://github.com/user-attachments/assets/fd0920ad-ea50-4357-a331-d60513fb6c08)

```python
import numpy as np
class Solution(object):

    def minimumDeleteSum(self, s1, s2):
        """
        :type s1: str
        :type s2: str
        :rtype: int
        """
        dp =  np.zeros((len(s2),len(s1)),dtype = int)
        self.ansmatrix(dp,s1,s2)
        longeststr = self.dflongsetstr(dp,s1)
        print "\n"
        return self.strminus(s1,s2,longeststr)



    #參數矩陣（同個字母為當前ascii，否則0）    
    def ansmatrix(self, dp,s1,s2):
        for i in range(len(s2)):
            for j in range(len(s1)):
                if s2[i] == s1[j]:
                    dp[i][j] = ord(s1[j])
                else :
                    dp[i][j] = 0
        print dp            


    def dflongsetstr(self, dp, s1):   
        longeststr = ""
        for i in range(len(dp)):
            # j 應該遍歷「欄」(columns)，所以要用 len(inputarray[0])
            for j in range(len(dp[0])):
                # np.max 必須傳入一個 list []
                # 取得前一個狀態，如果是第一列或第一欄就當作 0 或負無限大
                prev_val = dp[i-1][j-1] if (i > 0 and j > 0) else -10**9
                up_val = dp[i-1][j] if i > 0 else -10**9
                left_val = dp[i][j-1] if j > 0 else -10**9

                dp[i][j] = max(dp[i][j], dp[i][j] + prev_val, up_val, left_val)       
                      
        print dp   
        return dp[i][j]    

    #計算相減的值
    def strminus(self, s1, s2, lstr):

        total_delete_sum = sum(ord(c) for c in s1) + sum(ord(c) for c in s2) - 2 * lstr
        return total_delete_sum
```
