# 26/1/13
# 85. Maximal Rectangle

Given a rows x cols binary matrix filled with 0's and 1's, find the largest rectangle containi
Given a rows x cols binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

 <img width="402" height="322" alt="image" src="https://github.com/user-attachments/assets/962ae7c6-6b3b-4215-9e7c-3aeb678d61c7" />

Example 1:


Input: matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 6
Explanation: The maximal rectangle is shown in the above picture.
給定一個由's 和's 填充的rows x cols 二進位文件，找到只包含's 的最大矩形並返回其面積matrix

輸入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
Output: 6

### 這題的解法是先將橫竪的連續數量加起來 遇到0則重新計算 然後會得到一個橫著看和竪著看的最大1*n 或者 n*1的長方形 但是還是會出現2*2 以上的長方形及正方形 這時候就是去每一個格子做修正 具體判斷邏輯是找出這點往左及往上的最大來連續矩形 

![66a6288f-494d-4e10-ba74-9f702b699eb2](https://github.com/user-attachments/assets/c8542869-40c1-4d79-86f4-44de4bdfe621)

### 碰到一個問題就是中間可能會出現類似工字形 因爲只有計算頭尾的長寬 解法就是往上遍歷時都往左邊去檢查是否有0 動態修改minimal_wide
```python
import numpy as np

class Solution(object):
    def maximalRectangle(self, matrix):
        """
        :type matrix: List[List[str]]
        :rtype: int
        """
        if not matrix or not matrix[0]:
            return 0
            
        rows = len(matrix)
        cols = len(matrix[0])

        # --- 新增條件：如果全部都是 "1"，直接輸出總面積 ---
        all_ones = True
        for r in range(rows):
            for c in range(cols):
                if matrix[r][c] == "0":
                    all_ones = False
                    break
            if not all_ones:
                break
        
        if all_ones:
            return rows * cols

        # 1. 執行預處理，取得初始最大高度/寬度矩陣
        ansmatrix = self.horizontalandvertical(matrix)
        # 2. 執行核心 L型演算法結算面積
        return self.fixansmatrix(ansmatrix)   
        
    def horizontalandvertical(self, matrix):
        rows = len(matrix)
        cols = len(matrix[0])

        horizontalmatrix = [[0 for _ in range(cols)] for _ in range(rows)]
        verticalmatrix = [[0 for _ in range(cols)] for _ in range(rows)]
        
        # 垂直連續累加 (高度)
        for i in range(rows):
            for j in range(cols):
                if matrix[i][j] == "0": 
                    horizontalmatrix[i][j] = 0
                else:
                    horizontalmatrix[i][j] = 1 if i == 0 else horizontalmatrix[i-1][j] + 1

        # 水平連續累加 (寬度)
        for i in range(rows):
            for j in range(cols):
                if matrix[i][j] == "0": 
                    verticalmatrix[i][j] = 0
                else:        
                    verticalmatrix[i][j] = 1 if j == 0 else verticalmatrix[i][j-1] + 1

        maxmatrix = [[0 for _ in range(cols)] for _ in range(rows)]
        for i in range(rows):
            for j in range(cols):
                maxmatrix[i][j] = max(horizontalmatrix[i][j], verticalmatrix[i][j])
        
        print "--- Initial Max Matrix ---"
        for row in maxmatrix:
            print row
            
        return maxmatrix

    def fixansmatrix(self, matrix):
        rows = len(matrix)
        cols = len(matrix[0])
        m = [[int(matrix[i][j]) for j in range(cols)] for i in range(rows)]

        def is_valid(r, c):
            if r <= 0 or c <= 0: return False
            return m[r][c] != 0 and m[r-1][c] != 0 and \
                   m[r][c-1] != 0 and m[r-1][c-1] != 0

        for i in range(rows):
            for j in range(cols):
                if i == 0 or j == 0: continue
                
                if is_valid(i, j):
                    # --- 1. 往左 (pw) 拿到基礎寬度方塊數 ---
                    pw_base_count, curr_j = 0, j
                    while is_valid(i, curr_j):
                        curr_j -= 1
                        pw_base_count += 1
                    
                    # --- 2. 向上爬：每一層檢查瓶頸寬度 ---
                    curr_i = i
                    h_count = 0 
                    bottleneck_w_count = pw_base_count 
                    
                    while is_valid(curr_i, j):
                        h_count += 1
                        current_ul_count, temp_j = 0, j
                        while is_valid(curr_i, temp_j):
                            temp_j -= 1
                            current_ul_count += 1
                        
                        bottleneck_w_count = min(bottleneck_w_count, current_ul_count)
                        area = (h_count + 1) * (bottleneck_w_count + 1)
                        m[i][j] = max(m[i][j], area)
                        curr_i -= 1

                    # --- 3. 往左走：每一欄檢查瓶頸高度 ---
                    curr_j = j
                    w_count = 0
                    ph_base_count = 0
                    temp_i = i
                    while is_valid(temp_i, j):
                        temp_i -= 1
                        ph_base_count += 1
                        
                    bottleneck_h_count = ph_base_count
                    while is_valid(i, curr_j):
                        w_count += 1
                        current_lu_count, temp_i = 0, i
                        while is_valid(temp_i, curr_j):
                            temp_i -= 1
                            current_lu_count += 1
                        
                        bottleneck_h_count = min(bottleneck_h_count, current_lu_count)
                        area = (w_count + 1) * (bottleneck_h_count + 1)
                        m[i][j] = max(m[i][j], area)
                        curr_j -= 1

        print "--- Final Updated Matrix (m) ---"
        for row in m:
            print row
            
        return max(max(row) for row in m) if m else 0
```
