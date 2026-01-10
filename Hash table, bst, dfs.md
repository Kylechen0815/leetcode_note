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
