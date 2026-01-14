# 1/14
# 3453. Separate Squares I
You are given a 2D integer array squares. Each squares[i] = [xi, yi, li] represents the coordinates of the bottom-left point and the side length of a square parallel to the x-axis.

Find the minimum y-coordinate value of a horizontal line such that the total area of the squares above the line equals the total area of the squares below the line.

Answers within 10-5 of the actual answer will be accepted.

Note: Squares may overlap. Overlapping areas should be counted multiple times.

<img width="189" height="176" alt="image" src="https://github.com/user-attachments/assets/98851f67-81de-4ddd-97d4-fc3f50df5b9f" />


Example 1:

Input: squares = [[0,0,1],[2,2,1]]

Output: 1.00000

Explanation:

Any horizontal line between y = 1 and y = 2 will have 1 square unit above it and 1 square unit below it. The lowest option is 1.

給定一個二維整數數組squares。每個元素表示平行於 x 軸的正方形的左下角點的座標和邊長。squares[i] = [xi, yi, li]

求水平線的最小y 座標值，使得該線以上正方形的總面積等於該線以下正方形的總面積。

答案與正確答案誤差在某一範圍內均可接受。10-5

注意：方格可能重疊。重疊區域應多次計數。


例1：

輸入： squares = [[0,0,1],[2,2,1]]

輸出： 1.00000

解釋：

任意水平線之間的任何位置y = 1，y = 2其上方和下方都將各有 1 個方格單位。最小值是 1。

## solution:

##  這是一題很簡單的二元搜尋題目，不斷利用比較上下面積來收斂水平綫最終的位置

<img width="765" height="767" alt="image" src="https://github.com/user-attachments/assets/9f0ffa16-cc95-4753-a566-6aa568f3bc1a" />

```python
class Solution(object):
    def separateSquares(self, squares):
        """
        :type squares: List[List[int]]
        :rtype: float
        """
        return self.calculate(squares)
        

    def calculate(self, squares):
        if not squares:
            return 0.0  # 修正 1：必須回傳數字，不能只寫 return

        minvalue = 0
        maxvalue = 0
        base = []
        ceil = []

        for row in squares:
            height = row[1]
            current_ceil = row[1] + row[2] 
            base.append(height)
            ceil.append(current_ceil)

        # 這裡保留你原本找邊界的邏輯
        # 修正：minvalue 初始值建議設為第一筆資料，否則若資料都大於 0，min 永遠是 0
        minvalue = min(base)
        maxvalue = max(ceil)

        # 初始化二分搜尋的邊界
        low = float(minvalue)
        high = float(maxvalue)
        half = (low + high) / 2

        # 修正 2：while 條件不能用 areaup != 0，因為初始就是 0
        # 使用固定次數 (100次) 是處理浮點數最穩定的方式
        for _ in range(100):
            areaup = 0.0
            areadown = 0.0
            
            for row in squares:
                # 你原本的變數：row[1] 是底部, row[2] 是邊長
                # 修正 3：統一變數名稱，將 uparea/downarea 改回 areaup/areadown
                downheight = half - row[1]
                upheight = (row[1] + row[2]) - half

                if downheight >= row[2]: # 整個在下方
                    areadown += row[2] * row[2]
                elif upheight >= row[2]: # 整個在上方
                    areaup += row[2] * row[2]
                else:
                    # 修正 4：被切到時，面積應該是 邊長 * 截斷的高度
                    if downheight > 0:
                        areadown += row[2] * downheight
                    if upheight > 0:
                        areaup += row[2] * upheight

            # 修正 5：二分搜尋必須更新邊界（low 或 high），只動 half 會無法收斂
            if areaup > areadown:
                low = half
            else:
                high = half
            
            half = (low + high) / 2
                
        return float(half) # 確保回傳的是 double/float
                

       


```









任意水平線之間的任何位置y = 1，y = 2其上方和下方都將各有 1 個方格單位。最小值是 1。
