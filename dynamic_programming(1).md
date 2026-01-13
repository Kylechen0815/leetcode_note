# 26/1/9
## Return the maximum dot product between non-empty subsequences of nums1 and nums2 with the same length.
A subsequence of a array is a new array which is formed from the original array by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, [2,3,5] is a subsequence of [1,2,3,4,5] while [1,5,3] is not).

給定兩個數組nums1 和nums2。

傳回nums1 和 nums2 中長度相同的非空子序列的最大内積 。

數組的子序列是指從原始數組中刪除一些（也可以不刪除）字符，但不會改變剩餘字符相對位置而得到的新數組。 （例如， 是 數組[2,3,5] 的子序列，  而不是）。[1,2,3,4,5][1,5,3]

Example 1:

Input: nums1 = [2,1,-2,5], nums2 = [3,0,-6]
Output: 18
Explanation: Take subsequence [2,-2] from nums1 and subsequence [3,-6] from nums2.
Their dot product is (2*3 + (-2)*(-6)) = 18.

例1：

輸入： nums1 = [2,1,-2,5], nums2 = [3,0,-6]
輸出： 18
說明：從 nums1 中取出子序列 [2,-2]，從 nums2 中取出子序列 [3,-6]。
它們的内積是（2*3 + (-2)*(-6)）= 18。

### 這題是在考dynamic progamming， 解法是把目前這格定義為 上面一格 左邊一格 和本格和左上格的和 的最大值比較
![ab20dcbd-7d4b-458e-a1c9-0779cf993eab](https://github.com/user-attachments/assets/0415b35d-6e32-4139-afd7-8eb329ba4c27)
## solution：
### 這種解法錯在他不一定是往右下加， 也有可能往右下的右邊加 這樣就答案就不正確 

![609504821_1581432632987116_7449184954130108115_n](https://github.com/user-attachments/assets/78efe026-fc0b-4f22-acea-2ccc306a102e)

```python
import numpy as np

class Solution(object):
    def maxDotProduct(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: int
        """
        # 建立 len(nums1) x len(nums2) 的全 0 矩陣
        dp = np.zeros((len(nums2), len(nums1)), dtype=int)
        # 設為整數 -1,000,000,000
        dp2 = np.full((len(nums1), len(nums2)), -10**9, dtype=int)
        # range() 內必須是長度 (整數)
        for i in range(len(nums2)):
            for j in range(len(nums1)):
                # 執行點積運算並存入矩陣
                dp[i][j] = nums2[i] * nums1[j]
        
        
        print(dp)
        print("\n")
        self.ComMax(dp,dp2)

        
        # 根據題目要求，最後需要回傳一個整數結果（此處示範回傳矩陣最大值）
        return int(np.max(dp))

    def ComMax(self, inputarray, ansarray):
        # i 遍歷「列」(rows)
        for i in range(len(inputarray)):
            # j 應該遍歷「欄」(columns)，所以要用 len(inputarray[0])
            for j in range(len(inputarray[0])):
                # np.max 必須傳入一個 list []
                # 取得前一個狀態，如果是第一列或第一欄就當作 0 或負無限大
                prev_val = inputarray[i-1][j-1] if (i > 0 and j > 0) else -10**9
                up_val = inputarray[i-1][j] if i > 0 else -10**9
                left_val = inputarray[i][j-1] if j > 0 else -10**9

                inputarray[i][j] = max(inputarray[i][j], inputarray[i][j] + prev_val, up_val, left_val)
                
        
        print(inputarray)
```  
