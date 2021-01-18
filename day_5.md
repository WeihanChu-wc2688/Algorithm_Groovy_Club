# day5_二维bfs

---

## Leetcode Review

---

[429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)

```
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""
##-----
##bfs层级遍历搭配deque,注意deque.extend只能用于此种多分叉层级遍历不适用二叉树
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: Node
        :rtype: List[List[int]]
        """
        if not root:
            return []
        q = collections.deque([root])
        res = []
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                q.extend(node.children)
            res.append(level)
            
        return res
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[993. Cousins in Binary Tree](https://leetcode.com/problems/cousins-in-binary-tree/)

```
##------------
##建立字典，把每个节点的数值作为key，[level,parent] 作为value，用bfs的队列配合字典存储x,y和之前的所有结点信息，最后通过x,y作为索引搜索字典中配对的level是否相同，parent是否不同
class Solution(object):
    def isCousins(self, root, x, y):
        """
        :type root: TreeNode
        :type x: int
        :type y: int
        :rtype: bool
        """
        if not root:
            return False
        q = collections.deque()
        res = collections.defaultdict()
        q.append([root,0,0])
        
        while q:
            root,level,parent = q.popleft()
            res[root.val] = [level,parent]
            ##-----------
            ##early stoping:先判断是否找到x,y再进行后续操作，优化空间复杂度
            if x in res and y in res:
                break
            if root.left:
                q.append([root.left,level+1,root.val])
            if root.right:
                q.append([root.right,level+1,root.val])
        if res[x][0] == res[y][0] and res[x][1] != res[y][1]:
            return True
        return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[1490. Clone N-ary Tree](https://leetcode.com/problems/clone-n-ary-tree/)

```
##---
##clone graph的翻版，一模一样
class Solution(object):
    def cloneTree(self, root):
        """
        :type root: Node
        :rtype: Node
        """
        if not root:
            return None
        ##要把root变成list,因为他带val和数值
        q = deque([root])
        res = {}
        res[root] = Node(root.val,[])
        while q:
            node = q.popleft()
            for child in node.children:
                res[child] = Node(child.val,[])
                q.append(child)
                ##注意要在for循环里面更新克隆字典！！外面加的话child局部变量已经结束了不知道加什么
                res[node].children.append(res[child])
        return res[root]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[261. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)

```
class Solution:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """
    ##树得本质：无环形结构；
    ##无环形结构得特征：path = 总结点-1
    ##所以全局目标为判断path得长度是否为总结点-1
    ##先通过n*n的adjacent matrix建立每一个结点得相邻结点矩阵（相邻为1，不相邻为0）
    ##再通过bfs来寻找path长度最终进行判断
    ##用visit来记录访问次数
    def validTree(self, n, edges):
        numOfEdge = len(edges)
        # 判断是否为 (n - 1) 条边
        if numOfEdge != n - 1:
            return False
        # adjacent[i]里存i的相邻点
        adjacent = [[0] * n for _ in range(n)] 
        for i in range(numOfEdge):
            u = edges[i][0]
            v = edges[i][1]
            adjacent[u][v] = adjacent[v][u] = 1
        # visit[i]记录i是否被访问
        visit = [0] * n
        # 0作为根结点，开始向下遍历
        visit[0] = 1
        root, numOfVisited = 0, 1
        q = collections.deque()
        q.append(root)
        while len(q) != 0:
            root = q.popleft()
            for i in range(n):
                if adjacent[root][i] != 1:
                    continue
                # 如果相邻且没有被访问过，说明是儿子，加入队列
                if visit[i] == 0:
                    visit[i] = 1
                    numOfVisited += 1
                    q.append(i)
        if numOfVisited == n:
            return True
        return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[133. Clone Graph](https://leetcode.com/problems/clone-graph/)

```
##clone的意思就是通过helper给的ood function来创建一个新的相同数值和结构的图，而不是直接复制一样得数值存储在相同的reference里。
##所以通过搭配deque和dict进行dfs，每一步都把新的neighbor不断加进来
from collections import deque
class Solution(object):
    def cloneGraph(self, node):
        """
        :type node: Node
        :rtype: Node
        """
        if not node:
            return None
        ##进行bfs的字典
        visited = {}
        ##进行bfs的队列，先把第一个原结点存进来
        q = deque([node])
        #克隆字典：把结点作为索引，克隆node节点，把[结点数值，邻居]作为克隆节点的value与索引对应
        visited[node] = Node(node.val,[])
        ##进行bfs
        while q:
            n = q.popleft()
            ##遍历pop出来的原结点的邻居
            for neighbor in n.neighbors:
                ##如果邻居结点是新的，没被克隆过（不在克隆字典里），就同时克隆邻居节点存进克隆字典和queue等下一个循环访问；
                if neighbor not in visited:
                    visited[neighbor] = Node(neighbor.val,[])
                    q.append(neighbor)
                ##把刚才所有克隆过的邻居节点加入克隆字典中当前节点的邻居list中，因为初始克隆时默认邻居为空，要用for循环遍历queue中原节点的邻居进行复制
                visited[n].neighbors.append(visited[neighbor])
        #返回克隆字典
        return visited[node]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

```
from collections import deque
class Solution:
    """
    @param grid: a boolean 2D matrix
    @return: an integer
    """
    def numIslands(self, grid):
        if not grid or not grid[0]:
            return 0
        
        islands = 0
        ##逐次遍历grid中数值，如果发现岛屿（1）就进行bfs搜索连通块直到找完，同时计数器+1
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == '1':
                    self.bfs(grid, i, j)
                    islands += 1
                    
        return islands                    
    
    def bfs(self, grid, x, y):
        queue = deque([(x, y)])
        grid[x][y] = '0'
        #通过队列加入第一个岛屿1的坐标并pop出来对其进行四个方向（孩子）的遍历，借助is_valid判断是否为连通块，如果不是换方向（continue),如果是就在队列中加入连通块坐标并在grid中将次坐标值改为0（标记为已经访问过，否则会发生死循环不停的搜索这个为1的点）
        while queue:
            x, y = queue.popleft()
            for delta_x, delta_y in [(1, 0), (0, -1), (-1, 0), (0, 1)]:
                next_x = x + delta_x
                next_y = y + delta_y
                if not self.is_valid(grid, next_x, next_y):
                    continue
                queue.append((next_x, next_y))
                grid[next_x][next_y] = '0'
        #是否为连通块进行判断（坐标范围是否合法，此坐标的点是否为连通块（1））        
    def is_valid(self, grid, x, y):
        n, m = len(grid), len(grid[0])
        return 0 <= x < n and 0 <= y < m and grid[x][y] == '1'
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[133. Clone Graph](https://leetcode.com/problems/clone-graph/)

```
##clone的意思就是通过helper给的ood function来创建一个新的相同数值和结构的图，而不是直接复制一样得数值存储在相同的reference里。
##所以通过搭配deque和dict进行dfs，每一步都把新的neighbor不断加进来
from collections import deque
class Solution(object):
    def cloneGraph(self, node):
        """
        :type node: Node
        :rtype: Node
        """
        if not node:
            return None
        ##进行bfs的字典
        visited = {}
        ##进行bfs的队列，先把第一个原结点存进来
        q = deque([node])
        #克隆字典：把结点作为索引，克隆node节点，把[结点数值，邻居]作为克隆节点的value与索引对应
        visited[node] = Node(node.val,[])
        ##进行bfs
        while q:
            n = q.popleft()
            ##遍历pop出来的原结点的邻居
            for neighbor in n.neighbors:
                ##如果邻居结点是新的，没被克隆过（不在克隆字典里），就同时克隆邻居节点存进克隆字典和queue等下一个循环访问；
                if neighbor not in visited:
                    visited[neighbor] = Node(neighbor.val,[])
                    q.append(neighbor)
                ##把刚才所有克隆过的邻居节点加入克隆字典中当前节点的邻居list中，因为初始克隆时默认邻居为空，要用for循环遍历queue中原节点的邻居进行复制
                visited[n].neighbors.append(visited[neighbor])
        #返回克隆字典
        return visited[node]
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[860. Number of Distinct Islands](https://www.lintcode.com/problem/number-of-distinct-islands/description)

```
DIRECTIONS = [(1, 0), (-1, 0), (0, -1), (0, 1)]
from collections import deque
class Solution:
    def numberofDistinctIslands(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if not grid:
            return 0
        #建立空队列准备bfs,空字典准备判断是否重复，ans计数空字典加入岛屿次数即为结果
        queue, check, ans = deque(), set(), 0
        #双层for循环遍历grid，如果i,j处为岛屿的话立刻加入队列进行bfs，同时在grid里变为0代表访问过
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == 1:
                    #path为此岛屿的连通结构，所以每次循环到一个岛屿都要变为空进行bfs后填充连通的岛屿
                    path = " "
                    queue.append((i,j))
                    grid[i][j] = 0
                    while queue:
                        x,y = queue.popleft()
                        for dx, dy in DIRECTIONS:
                            new_x, new_y = x+dx, y+dy
                            #如果是合法岛屿，立刻加入队列并在grid标记为0代表访问过
                            if self.is_valid(grid,new_x, new_y):
                                queue.append((new_x, new_y))
                                grid[new_x][new_y]= 0
                                #从i,j点起，每次添加新位置和老位置走过的距离（路径）来记录连通块形状
                                path += str(new_x-i) + str(new_y-j)
                    #判断路径在check里有没有一样得，根据set的无序性质如果路径距离相同就算顺序不同也是相同连通块，所以可以通过这种方式甄别是否为distinct island!
                    if path not in check:
                        ans += 1
                        check.add(path)
        return ans
    #判断合法岛屿：x,y大于0且此处为1    
    def is_valid(self,grid,x,y):
        row, col = len(grid), len(grid[0])
        return x >=0 and x <row and y >=0 and y < col and grid[x][y]==1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---

[542. 01 Matrix](https://leetcode.com/problems/01-matrix/)

```
from collections import deque
class Solution(object):
    def updateMatrix(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: List[List[int]]
        """
        visited = set()
        q = deque()
        #双层for循环把所有0点坐标存入visited的set和队列准备bfs
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == 0:
                    visited.add((i,j))
                    q.append((i,j))
        #开始bfs            
        while q:
            x,y = q.popleft()
            #遍历上下左右四个方向，对四个方向得新坐标依次处理
            for dir in [(1,0),(-1,0),(0,1),(0,-1)]:
                newX,newY = x+dir[0], y+dir[1]
                #如果新方向值为1且坐标合法：此处值+1，因为之前得x,y是从队列出来的0点坐标，意味着这个坐标是0点相邻的所以要+1，然后把坐标加入visited和更新表示已处理过（因为离处理过得最近所以+1即可）；以此类推，如果队列进入得是上一个处理过得点，就继续+1，直到遍历完所有的点，matrix就变为了每个点记录离0距离的matrix！
                if newX >=0 and newX <= len(matrix)-1 and newY >=0 and newY <= len(matrix[0])-1 and (newX,newY) not in visited:
                    
                    matrix[newX][newY] = matrix[x][y] + 1
                    visited.add((newX,newY))
                    q.append((newX,newY))
        return matrix
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[1091. Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)

```
class Solution(object):
    def shortestPathBinaryMatrix(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
##思路：首先判断矩阵两端是否为1，如果是直接返回-1；
##      然后bfs双层遍历整个矩阵，对每个点的8个方向bfs判断是否为合法点而且为0，如果为0就改为1作为访问标记，并且在q中加入此坐标，在d中+1记录步数，遍历到最后一个结点时返回d作为结果，但如果无法遍历到最后一个结点（中途死胡同）那么最后要返回-1！！！
    
        n = len(grid)
        if grid[0][0]==1 or grid[n-1][n-1]==1:
            return -1
        q = [(0, 0, 1)]
        grid[0][0] = 1
        for i, j, d in q:
            if i == n-1 and j == n-1: return d
            for x, y in ((i-1,j-1),(i-1,j),(i-1,j+1),(i,j-1),(i,j+1),(i+1,j-1),(i+1,j),(i+1,j+1)):
                if 0 <= x < n and 0 <= y < n and grid[x][y]==0:
                    grid[x][y] = 1
                    q.append((x, y, d+1))
        return -1 
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

```
##思路：既然翻转所有被包围的O，那么只需要先找到四边的O暂时换成W，然后把剩下所有的O换成X再把四边的W换回O，因为只有四条边上的O不会被X包围，用bfs一层一层以方形朝里缩进遍历完所有的点处理。
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        #如果任何一个Board为空直接返回
        if not any(board):
            return

        n, m = len(board), len(board[0])
        #用k作为长宽的最大一侧，先遍历range(k)再遍历四条边关于k的坐标取得四条边上的所有坐标
        q = [ij for k in range(max(n,m)) for ij in ((0, k), (n-1, k), (k, 0), (k, m-1))]
        #用q开始bfs
        while q:
            i, j = q.pop()
            #如果ij坐标合法而且为O就把它换成W，然后把四周的点加进q继续遍历，朝matrix里缩进
            if 0 <= i < n and 0 <= j < m and board[i][j] == 'O':
                board[i][j] = 'W'
                #要更新q
                q += (i, j-1), (i, j+1), (i-1, j), (i+1, j)
                
        for i in range(n):
            for j in range(m):
                if board[i][j] == 'O':
                    board[i][j] = 'X'
                elif board[i][j] == 'W':
                    board[i][j] = 'O'
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[490. The Maze](https://leetcode.com/problems/the-maze/)

```
class Solution(object):
    ##经典bfs，只不过在孩子那里要一直循环到出口为1才停止
    def hasPath(self, maze, start, destination):

        # write your code here

        Q = [start]

        n = len(maze)

        m = len(maze[0])

        dirs = ((0, 1), (0, -1), (1, 0), (-1, 0))

        while Q:

            i, j = Q.pop(0)

            maze[i][j] = 2

            if i == destination[0] and j == destination[1]:

                return True

            for x, y in dirs:

                row = i + x

                col = j + y
                #只有循环到碰壁才会出来

                while 0 <= row < n and 0 <= col < m and maze[row][col] != 1:

                    row += x

                    col += y
                #因为碰壁后是过一个的，所以要在-x,-y返回到原来的难度
                row -= x

                col -= y
                #如果为0，加回到队列继续遍历
                if maze[row][col] == 0:

                    Q.append([row, col])

        

        return False
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[505. The Maze II](https://leetcode.com/problems/the-maze-ii/)

```
class Solution:
    def shortestDistance(self, maze, start, destination):
        """
        :type maze: List[List[int]]
        :type start: List[int]
        :type destination: List[int]
        :rtype: int
        """
        if not maze or not maze[0]:
            return -1
        seen = [[float('inf')]*len(maze[0]) for _ in range(len(maze))] # records the current shortest distance between start and destination
        seen[start[0]][start[1]] = 0
        q = collections.deque([(start[0],start[1],0)])
        while q:
            y, x, d = q.popleft()
            trav = y
            dist = d
            while trav > 0 and maze[trav-1][x] == 0:
                trav -= 1
                dist += 1
            if seen[trav][x] > dist:
                seen[trav][x] = dist
                q.append((trav, x, dist))
            trav = y
            dist = d
            while trav < len(maze)-1 and maze[trav+1][x] == 0:
                trav += 1
                dist += 1
            if seen[trav][x] > dist:
                seen[trav][x] = dist
                q.append((trav, x, dist))
            trav = x
            dist = d
            while trav > 0 and maze[y][trav-1] == 0:
                trav -= 1
                dist += 1
            if seen[y][trav] > dist:
                seen[y][trav] = dist
                q.append((y, trav, dist))
            trav = x
            dist = d
            while trav < len(maze[0])-1 and maze[y][trav+1] == 0:
                trav += 1
                dist += 1
            if seen[y][trav] > dist:
                seen[y][trav] = dist
                q.append((y, trav, dist))
        dy, dx = destination
        return seen[dy][dx] if seen[dy][dx] != float('inf') else -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
[1197. Minimum Knight Moves](https://leetcode.com/problems/minimum-knight-moves/)

```
##bfs模板，孩子按照象棋格子走
class Solution:
    def minKnightMoves(self, s: int, e: int) -> int:
        q = collections.deque()
        q.append([0,0,0])
        visited = set()
        visited.add((0,0))
        while q:
            x,y,steps = q.popleft()
            if x==s and y==e:
                return steps
            dirs = [(x-1,y-2),(x-2,y-1),(x-2,y+1),(x-1,y+2),(x+1,y-2),(x+2,y-1),(x+1,y+2),(x+2,y+1)]
            for i,j in dirs:
                if (i,j) not in visited:
                    q.append([i,j,steps+1])
                    visited.add((i,j))
        return -1
```
**Analysis**

Time Complexity: 

Space Complexity: 

---
