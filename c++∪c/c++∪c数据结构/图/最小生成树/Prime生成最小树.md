### 🎬 Prim 算法的“滚雪球”动画

Prim 的核心思想是：**从一个点出发，不断把离当前“圈子”最近的邻居拉进来。**

假设还是那 5 个岛的例子，Prim 是这样干活的：

1. **选个起点**：比如从 **岛 0** 开始。此时，岛 0 在“已通电圈子”里，其他岛在外面。
    
2. **看邻居**：岛 0 翻开抽屉，看到邻居有 2（距离 1）和 1（距离 5）。
    
3. **拉人进圈**：挑那个最近的！**岛 2** 进圈。【选中 0-2】
    
4. **扩大搜索**：现在“圈子”里有 0 和 2 了。它们一起翻抽屉：
    
    - 0 看到 1（距离 5）
        
    - 2 看到 1（距离 2）、3（距离 4）、4（距离 8）
        
5. **再拉人**：全圈子最便宜的是 2 到 1 的 **2**。**岛 1** 进圈！【选中 2-1】
    
6. **重复执行**：直到所有岛都在圈子里。
    

---

### ⚔️ Prim vs Kruskal：神仙打架

这两兄弟虽然最后修出来的路一模一样，但性格完全不同：

|**特性**|**Kruskal (克鲁斯卡尔)**|**Prim (普里姆)**|
|---|---|---|
|**策略**|**边**驱动（挑便宜的边）|**点**驱动（找最近的邻居）|
|**手段**|排序 + 并查集|优先队列（类似 Dijkstra）|
|**擅长领域**|**稀疏图**（路很少，挑起来快）|**稠密图**（路非常多，点连点快）|

---

### 💻 怎么写 Prim 的代码？

你会发现 Prim 的代码和你昨天写的 **Dijkstra** 简直是“亲兄弟”！

- **Dijkstra**：找的是“离**起点**最近的点”。
    
- **Prim**：找的是“离**当前整个圈子**最近的点”。
    

代码里唯一的区别就在于“更新账本”的那一行：

- Dijkstra: `dist[u] + weight < dist[v]` (累加距离)
    
- Prim: `weight < dist[v]` (只看当前的这根线够不够便宜)
    

---

### 🛠️ Prim 核心代码片段

因为你已经写过 Dijkstra 了，看这段代码你会觉得非常亲切：

C++

```
void Prim(Graph *graph, int startVertex) {
    int n = graph->numvertexNum;
    vector<int> dist(n, INF);    // 记录每个点离“圈子”的最短距离
    vector<bool> inMST(n, false); // 记录谁已经在圈子里了
    
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;

    dist[startVertex] = 0;
    pq.push({0, startVertex});

    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();

        if (inMST[u]) continue;
        inMST[u] = true; // 拉入圈子

        Node *temp = graph->adjacencyList[u];
        while (temp) {
            int v = temp->vertex;
            int w = temp->weight;

            // 如果 v 还没进圈，且这根线比之前记录的都要便宜
            if (!inMST[v] && w < dist[v]) {
                dist[v] = w; // 更新账本：记录这个点离圈子的最短距离
                pq.push({dist[v], v});
            }
            temp = temp->next;
        }
    }
}
```

---

