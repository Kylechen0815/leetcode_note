# 26/1/10 (solution needed improved)
## Smallest Subtree with all the Deepest Nodes

Given the root of a binary tree, the depth of each node is the shortest distance to the root.

Return the smallest subtree such that it contains all the deepest nodes in the original tree.

A node is called the deepest if it has the largest depth possible among any node in the entire tree.

The subtree of a node is a tree consisting of that node, plus the set of all descendants of that node.

Input: root = [3,5,1,6,2,0,8,null,null,7,4]
Output: [2,7,4]
Explanation: We return the node with value 2, colored in yellow in the diagram.
The nodes coloured in blue are the deepest nodes of the tree.
Notice that nodes 5, 3 and 2 contain the deepest nodes in the tree but node 2 is the smallest subtree among them, so we return it.

 
<img width="722" height="614" alt="sketch1" src="https://github.com/user-attachments/assets/b52b3f86-ecb5-42bd-aed8-ab88ca25b942" />


給定root一棵二元樹，每個節點的深度是到根節點的最短距離。

傳回包含原樹中所有最深節點的最小子樹。

如果一個節點在整棵樹中具有最大的深度，則稱該節點為最深節點。

一個節點的子樹是由該節點及其所有後代節點所構成的樹。

例1：

輸入： root = [3,5,1,6,2,0,8,null,null,7,4]
輸出： [2,7,4]
說明：我們傳回值為 2 的節點，圖中黃色標記的節點。
藍色標記的節點是樹中最深的節點。
請注意，節點 5、3 和 2 包含樹中最深的節點，但節點 2 是其中最小的子樹，因此我們返回它。

### 這題要求在一顆bst找到最深節點的共同子樹，目前的方法是先找到所有的最深點，然後把這些點的路徑存起來，再做相同比較，如果有相同的節點表示這些子樹不是最小值，將目前的node刪除，往下檢查。
### 

```python
class Solution(object):
    def subtreeWithAllDeepest(self, root):
        if not root: return None
        
        self.dp = []
        # 1. 找出所有葉子節點及其深度
        self.deepestnode(root, 1, self.dp)
        
        # 2. 只保留最深層的節點
        self.deletenode(self.dp)
        
        # 3. 找出通往最深節點的物件路徑
        all_path = self.findnode(root, self.dp)
        
        # 4. 取得分歧點後的路徑（如果是單一路徑，會剩下最後一個節點）
        all_path2 = self.common_route(all_path)
        
        # 5. 回傳該路徑的第一個節點（即為 LCA）
        return all_path2[0][0]

    def deepestnode(self, curr, level, dp):
        if curr is None: return
        if curr.left is None and curr.right is None:
            dp.append([level, curr.val])
        else:
            self.deepestnode(curr.left, level + 1, dp)
            self.deepestnode(curr.right, level + 1, dp)

    def deletenode(self, dp):
        if not dp: return
        max_level = max(row[0] for row in dp)
        dp[:] = [row for row in dp if row[0] == max_level]

    def findnode(self, root, dp):
        targets = set([row[1] for row in dp])
        all_paths = []

        def dfs(node, current_path):
            if not node: return
            current_path.append(node) # 存入節點物件
            if node.val in targets:
                all_paths.append(list(current_path))
            dfs(node.left, current_path)
            dfs(node.right, current_path)
            current_path.pop()

        dfs(root, [])
        return all_paths

    def common_route(self, all_path):
        # 移除掉原本的 if len(all_path) == 1 判斷，讓單一路徑也能正常 pop
        while len(all_path[0]) > 1:
            next_node = all_path[0][1]
            all_match = True
            for p in all_path:
                if len(p) <= 1 or p[1] != next_node:
                    all_match = False
                    break
            
            if all_match:
                for p in all_path:
                    p.pop(0)
            else:
                break
        return all_path
```
