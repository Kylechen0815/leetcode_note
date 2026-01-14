# 26/1/14
# 84. Largest Rectangle in Histogram

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

<img width="522" height="242" alt="image" src="https://github.com/user-attachments/assets/46d4b20a-db44-47a1-89d6-a8f3941a69fb" />

Example 1:

Input: heights = [2,1,5,6,2,3]

Output: 10

Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

給定一個整數數組，heights表示直方圖的柱形高度，其中每個柱形的寬度為1，傳回直方圖中最大矩形的面積。

例1:

輸入： heights = [2,1,5,6,2,3]
輸出： 10
說明：以上為直方圖，其中每個長條圖的寬度為 1。
圖中紅色區域顯示的是最大的矩形，其面積為 10 個單位。

## solution:
### 這題要求會運用monotonic stack ：也就是這個stack之内只會有按照遞增或遞減的方式排序數列， 所以需要做資料的替換以及篩選。
### monotonic stack的用處是可以很快速的找到最相鄰的較大值或較小值
### 這題有使用到這個技巧 方法是先利用list的第0項往右看作ms（若右邊沒有比當前值小的就回傳-1）
<img width="680" height="693" alt="image" src="https://github.com/user-attachments/assets/1ed71b02-2aaa-42e5-b6cc-c09df7f605a3" />

### 接下來就是算面積，算法是max(自己，區間内的最小值 * 目前已經數的條數， 自己 * 到上一個有index值的距離）
### 但是會出現一個問題就是它只能處理右圖左邊的部分， 右邊的部分會有多算的情況（後來改成少算，因爲參照標準只剩他一格，右邊那個比他小會有新的index)
### 後來采取左右都看得方式 就是從左至右和從右至左都做一個 上述圖表，算兩次的最大面積 如果左右看時，剛好高都是一樣 則合并面積

<img width="1182" height="642" alt="image" src="https://github.com/user-attachments/assets/f2aa94f1-e603-4d88-9798-3a0a0a0bfbf9" />

```python
class Solution(object):
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        stack = self.stack_up_list(heights)
        return self.calculate_max_area(stack)

    def stack_up_list(self,input_list):
        # 建立一個空的 Stack
        stack = []
        
        # 將 list 中的元素依序堆進去
        for item in input_list:
            stack.append(item)
            #print "Pushing: %s | Current Stack: %s" % (item, stack)
       
        return stack


 
    def calculate_max_area(self, data):
            n = len(data)
            if n == 0: return 0
            
            # 用來記錄每個點向左與向右能延伸的距離
            dist_l = [0] * n
            dist_r = [0] * n
            
            # --- 第一部分：從右向左掃描 (R2L) ---
            m_row_right = [0] * n
            stack_r = []
            count_r = 0
            for i in range(n - 1, -1, -1):
                val = data[i]
                if val == 0:
                    stack_r = []
                    count_r = 0
                    continue
                count_r += 1
                while len(stack_r) > 0 and stack_r[-1][0] >= val:
                    stack_r.pop()
                
                curr_mark_val = -1
                curr_mark_idx = -1
                prev_mark_idx = -1
                
                if len(stack_r) > 0:
                    curr_mark_val = stack_r[-1][0]
                    curr_mark_idx = stack_r[-1][1]
                    if len(stack_r) > 1:
                        prev_mark_idx = stack_r[-2][1]
                
                stack_r.append((val, i))
                min_mark_val = stack_r[0][0]
                
                if curr_mark_val == -1:
                    dist_r[i] = count_r
                    c1 = val
                    c2 = val * count_r
                    m_row_right[i] = max(c1, c2)
                else:
                    dist_to_curr = abs(curr_mark_idx - i)
                    dist_r[i] = dist_to_curr
                    min_in_gap = val 
                    c1 = val
                    c2 = val * dist_to_curr
                    c3 = min_in_gap * dist_to_curr
                    c4 = min_mark_val * count_r
                    c5 = 0
                    if prev_mark_idx != -1:
                        dist_to_prev = abs(prev_mark_idx - i)
                        c5 = dist_to_prev * curr_mark_val
                    m_row_right[i] = max(c1, c2, c3, c4, c5)

            # --- 第二部分：從左向右掃描 (L2R) ---
            m_row_left = [0] * n
            stack_l = []
            count_l = 0
            for i in range(n):
                val = data[i]
                if val == 0:
                    stack_l = []
                    count_l = 0
                    continue
                count_l += 1
                while len(stack_l) > 0 and stack_l[-1][0] >= val:
                    stack_l.pop()
                
                curr_mark_val = -1
                curr_mark_idx = -1
                prev_mark_idx = -1

                if len(stack_l) > 0:
                    curr_mark_val = stack_l[-1][0]
                    curr_mark_idx = stack_l[-1][1]
                    if len(stack_l) > 1:
                        prev_mark_idx = stack_l[-2][1]
                
                stack_l.append((val, i))
                min_mark_val = stack_l[0][0]
                
                if curr_mark_val == -1:
                    dist_l[i] = count_l
                    c1 = val
                    c2 = val * count_l
                    m_row_left[i] = max(c1, c2)
                else:
                    dist_to_curr = abs(curr_mark_idx - i)
                    dist_l[i] = dist_to_curr
                    min_in_gap2 = val 
                    c1 = val
                    c2 = val * dist_to_curr
                    c3 = min_in_gap2 * dist_to_curr
                    c4 = min_mark_val * count_l
                    c5 = 0
                    if prev_mark_idx != -1:
                        dist_to_prev = abs(prev_mark_idx - i)
                        c5 = dist_to_prev * curr_mark_val
                    m_row_left[i] = max(c1, c2, c3, c4, c5)

            # --- 第三部分：合併結果 ---
            max_val = 0
            for i in range(n):
                # 關鍵：將左右兩邊的距離合併（扣除重複算的自己）
                # 例如 val=5, 向左看距離 7, 向右看距離 6, 總寬度 = 7+6-1 = 12
                c_combined = data[i] * (dist_l[i] + dist_r[i] - 1)
                
                final_area = max(m_row_right[i], m_row_left[i], c_combined)
                max_val = max(max_val, final_area)
                
            return max_val
                    


```
