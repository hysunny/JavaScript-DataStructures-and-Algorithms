
> 简书地址: https://www.jianshu.com/p/9d9a16019859

### 1. 图的定义
> 图由`边`的集合及`顶点`的集合组成。
边由`顶点对`(v1, v2)定义，v1和v2分别是图中的两个顶点。
顶点也有`权重`，也称为成本。
图中的一系列顶点构成`路径`，路径中所有的顶点都由边连接。
路径的长度用路径中**第一个顶点**到**最后一个顶点**之间边的数量表示。

> 如果一个图的顶点对是有序的，则可以称之为有向图。有向图表明了顶点的流向。
![有向图.png](/images/directed-graph.png)

> 如果图是无序的，则称之为无序图，或无向图：
![无序图.png](/images/undirected-graph.png)

本篇主要讨论无序图。

> 表示图的边的方法称为`邻接表`或者`邻接数组`。
这种方法将边存储为由顶点的相邻顶点列表构成的数组，并以此顶点作为索引。
![邻接表.png](/images/adjacency-ist.png)


### 2. 图算法 —— 搜索图
（1）深度优先搜索
深度优先搜索包括从一条路径的起始顶点开始追溯，直到到达最后一个顶点，然后回溯，继续追溯下一条路径，直到到达最后的顶点，如此往复，直到没有路径为止。这不是在搜索特定的路径，而是通过搜索来查看在图中有哪些路径可以选择。
  > ![深度优先搜索.png](/images/dfs.png)

（2）广度优先搜索
广度优先搜索从第一个顶点开始，尝试尽可能靠近它的顶点。本质上，这种搜索在图上是**逐层**移动的，首先检查最靠近第一个顶点的层，再逐渐向下移动到离起始顶点最远的层。
广度优先算法使用了抽象队列来对已访问过的顶点进行排序。原理如下：
```
a. 查找与当前顶点相邻的未访问顶点，将其添加到已访问顶点列表及队列中；
b. 从图中取出下一个顶点v，添加到已访问的顶点列表
c. 将所有与v相邻的未访问顶点添加到队列
```
> ![广度优先搜索.png](/images/bfs.png)

### 3. 实现一个图类
```js
class Graph {

  constructor(v = 0, vertexList = []) {
    this.vertices = v // 顶点数
    this.edges = 0	// 边数
    this.adj = []		// 邻接数组
    this.marked = []	// 存储已访问的节点
    this.edgeTo = []	// 保存一个顶点到下一个顶点的所有边
    this.vertexList = vertexList	// 将各个顶点关联到一个符号
    // 为每个顶点初始化一个数组用来存储与他相连的节点
    for (let i = 0; i < this.vertices; i++) {
      this.adj[i] = []
    }
    // 初始化所有顶点为未访问过
    for (let i = 0; i < this.vertices; i++) {
      this.marked[i] = false
    }
  }
  
  // 添加一条边
  addEdge(v, w) {
    this.adj[v].push(w)
    this.adj[w].push(v)
    this.edges++
  }
  
  // 深度优先搜索
  // 访问一个没有被访问过的顶点，将它标记为已访问，再递归地去访问在初始顶点的邻接表中其他没有被访问顶点
  dfs(v = 0) {
    this.marked[v] = true
    if (this.adj[v] !== undefined) {
        console.log(`Visited vertex: ${v}`)
        this.adj[v].forEach(item => {
            if (!this.marked[item]) {
                this.dfs(item)
            }
        })
    }
  }
  
  // 广度优先搜索
  // (1) 查找与当前顶点相邻的未访问顶点，将其添加到已访问顶点列表及队列中
  // (2) 从图中取出下一个顶点v，添加到已访问的顶点列表中
  // (3) 将所有与v相邻的未访问顶点添加到队列中
  bfs(s = 0) {
    this.resetMarked()
    const queue = []
    this.marked[s] = true
    queue.push(s) // 添加到队尾
    while(queue.length > 0) {
      const v = queue.shift() // 从队首移除
      if (this.adj[v] !== undefined) {
        console.log(`Visited vertex: ${v}`)
        // 遍历访问与v的邻接表中其他没有被访问的顶点
        this.adj[v].forEach(item => {
          if (!this.marked[item]) {
            // 保存 item => v 的边
            this.edgeTo[item] = v
            // 将 item 标记为已访问
            this.marked[item] = true
            queue.push(item)
          }
        })
      }
    }
    this.resetMarked()
  }
  
  // 存储与指定顶点有共同边的所有顶点
  pathTo(v) {
    this.dfs()
    let source = 0
    if (!this.marked[v]) return undefined
    const path = []
    let i = v
    while  (i !== source) {
      if (this.edgeTo[i] !== undefined) {
        path.push(i)
        i = this.edgeTo[i]
      } else {
        i = source
      }
    }
    path.push(source)
    this.resetMarked()
    return path.reverse()
  }
  
  // 拓扑排序
  // 设置排序进程并调用一个辅助函数 topSortHelper()
  // 然后显示排序好的顶点列表
  topSort() {
    const stack = []
    const visited = []
    for (let i = 0; i < this.vertices; i++) {
      visited[i] = false
    }
    for (let i = 0; i < this.vertices; i++) {
      if (!visited[i]) {
        this.topSortHelper(i, visited, stack)
      }
    }
    for (let i = stack.length - 1; i >= 0; i--) {
      if (stack[i] !== undefined) {
        console.log(this.vertexList[stack[i]])
      }
    }
  }
  
  // 拓扑排序辅助函数
  // 将当前顶点标记为已访问，然后递归地访问当前顶点邻接表中每个相邻顶点，标记这些顶点为已访问。
  // 最后将当前顶点压入栈
  topSortHelper(v, visited, stack) {
    visited[v] = true
    this.adj[v].forEach(item => {
      if (!visited[item]) {
        this.topSortHelper(item, visited, stack)
      }
    })
    stack.push(v)
  }
  
  showGraph() {
    const visited = []
    for (let i = 0; i < this.vertices; i++) {
      console.log(' ')
      visited.push(this.vertexList[i])
      for (let j = 0; j < this.vertices; j++) {
        if (this.adj[i][j] !== undefined && visited.indexOf(this.vertexList[j] === -1)) {
          console.log(`${this.vertexList[i]} -> ${this.adj[i][j]}`)
        }
        visited.pop()
      }
    }
  }
  
  // 重置所有顶点为未访问过
  resetMarked() {
    for (let i = 0; i < this.vertices; i++) {
      this.marked[i] = false
    }
  }
}



// test
const lessons = ['CS1', 'CS2', 'Data Structures',
                     'Assembly Language', 'Operating Systems',
                     'Algorithms']
const g = new Graph(6, lessons)
g.addEdge(1, 2)
g.addEdge(2, 5)
g.addEdge(1, 3)
g.addEdge(1, 4)
g.addEdge(0, 1)

console.log('======= showGraph =======')
g.showGraph()

console.log('======= 深度优先 =======')
g.dfs()
// Visited vertex: 0
// Visited vertex: 1
// Visited vertex: 2
// Visited vertex: 5
// Visited vertex: 3
// Visited vertex: 4

console.log('======= 广度优先 =======')
g.bfs() 
// Visited vertex: 0
// Visited vertex: 1
// Visited vertex: 2
// Visited vertex: 3
// Visited vertex: 4
// Visited vertex: 5

console.log('拓扑排序 =======')
g.topSort() 
// CS1
// CS2 
// Operating Systems
// Assembly Language
// Data Structures
// Algorithms 

console.log('最短路径 =======')
const path = g.pathTo(4)
console.log(path) // [0, 1, 4]
```