# Solved problems
## 26/1/9
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

這題是在考dynamic progamming， 解法是把目前這格定義為 上面一格 左邊一格 和本格和左上格的和 的最大值比較
![ab20dcbd-7d4b-458e-a1c9-0779cf993eab](https://github.com/user-attachments/assets/0415b35d-6e32-4139-afd7-8eb329ba4c27)

這種解法錯在他不一定是往右下加， 也有可能往右下的右邊加 這樣就答案就不正確 

![609504821_1581432632987116_7449184954130108115_n](https://github.com/user-attachments/assets/78efe026-fc0b-4f22-acea-2ccc306a102e)

