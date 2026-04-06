### 🎬 Kruskal 算法的“挑便宜货”动画

假设我们手头有这样一张地图，上面有 4 个城市（0, 1, 2, 3），它们之间有 5 条可以修的路，每条路都有造价：

- 0 到 1，造价 **1**
    
- 1 到 2，造价 **2**
    
- 0 到 2，造价 **3**
    
- 2 到 3，造价 **4**
    
- 0 到 3，造价 **5**
    

Kruskal 算法的策略非常简单粗暴，只有三步：

#### 📍 第一幕：全网比价，按价格排序

我们把所有能修的路拉出来，像菜市场挑菜一样，按造价从便宜到贵排个序：

1. 0 - 1 (造价 1)
    
2. 1 - 2 (造价 2)
    
3. 0 - 2 (造价 3)
    
4. 2 - 3 (造价 4)
    
5. 0 - 3 (造价 5)
    

此时，4 个城市孤零零地立在地图上，谁也不认识谁。

#### 📍 第二幕：疯狂扫货，只要不形成环

我们从最便宜的开始挑：

- **挑中 0 - 1（造价 1）**：0 和 1 没连过，修！【当前总造价：1】
    
- **挑中 1 - 2（造价 2）**：把 1 和 2 连起来，现在 0, 1, 2 都连通了，修！【当前总造价：1 + 2 = 3】
    
- **挑中 0 - 2（造价 3）**：等等！0 和 2 之间如果再拉一根线，地图上 `0 - 1 - 2 - 0` 就形成了一个**闭合的环**。既然通过 1 它们本来就能通电，再花 3 块钱连它们就是浪费！**果断放弃！**
    
- **挑中 2 - 3（造价 4）**：把 2 和 3 连起来，3 号城市终于也通电了，修！【当前总造价：3 + 4 = 7】
    

#### ✨ 终局：大功告成

到这里，所有的城市（0, 1, 2, 3）都已经连在了一起，我们用了最便宜的边，总成本是 **7**。后面的 0 - 3（造价 5）直接不用看了，因为所有人都已经通电了。

---

### 🛡️ 高阶武器：并查集（Union-Find）

在上面的动画里，我们人类用肉眼一眼就能看出连上 `0 - 2` 会形成环。但是计算机是个近视眼，它只知道一条条的边，它怎么知道连上某条边会不会形成环呢？

为了解决这个难题，我们需要打造一个全新的高级数据结构辅助工具——**并查集**。

它的核心思想就是**“江湖认老大”**：

1. **初始状态**：每个城市都是一个独立的帮派，自己是自己的老大。
    
    - 0 的老大是 0
        
    - 1 的老大是 1
        
    - 2 的老大是 2
        
    - 3 的老大是 3
        
2. **合并帮派（Union）**：当我们决定修 `0 - 1` 这条路时，我们就让 0 的老大认 1 的老大做“顶头上司”。现在 0 和 1 属于同一个帮派了，它们的终极老大都是 1。
    
3. **查找老大（Find）**：当我们要考虑修 `0 - 2` 这条路时，计算机先去查 0 的终极老大（是 1），再去查 2 的终极老大（是 2）。
    
    - 发现它们的老大不一样（1 $\neq$ 2），说明它们处于不同的帮派，连接它们**不会形成环**！
        
4. **环的诞生**：如果我们接下来考虑 `1 - 2`，查 1 的老大是 2，查 2 的老大也是 2。
    
    - 计算机发现：**天哪，你们的终极老大居然是同一个人！** 这说明你们早就在一个帮派里连通了，如果现在再连你们，就会**形成环**！拒绝连接！
        

---

### 💻 怎么用 C++ 代码实现并查集？

并查集的代码非常短，短到让人不可思议，但逻辑极其精妙。它只需要一个一维数组和两个函数：

C++

```
#include <vector>
using namespace std;

// 并查集结构体
struct DisjointSet {
    vector<int> parent;

    // 初始化：每个人一开始的老大都是自己
    DisjointSet(int n) {
        parent.resize(n);
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    // 核心函数 1：找终极老大（带路径压缩的超级优化版）
    int find(int i) {
        if (parent[i] == i)
            return i;
        // 顺藤摸瓜找老大的老大，顺便把一路上所有人的老大都直接改成终极老大（路径压缩）
        return parent[i] = find(parent[i]); 
    }

    // 核心函数 2：合并两个帮派
    void unite(int i, int j) {
        int root_i = find(i);
        int root_j = find(j);
        if (root_i != root_j) {
            parent[root_i] = root_j; // 让一个老大认另一个老大当上级
        }
    }
};
```

有了这个并查集武器，Kruskal 算法就变成了：**把边按权重排序 ➡️ 用 `find` 判断老大人选 ➡️ 用 `unite` 连线。**
## 完整代码
```
#include <cctype>

#include<iostream>

#include<vector>

#include<algorithm>

using namespace std;

struct Edge {

    int u, v, weight;

};

  
  

bool compareEdges(Edge a, Edge b) {

    return a.weight < b.weight; // 从小到大排

}

//并查集结构体

struct disjointset{

    vector<int>parent;

    disjointset(int n){

        parent.resize(n);

        for(int i=0;i<n;i++){

            parent[i]=i;

        }

    }

    int find(int i)

    {

        if(parent[i]==i)

        return i;

        return parent[i]=find(parent[i]);

    }

    void unint(int i,int j)

    {

        int root_i=find(i);

        int root_j=find(j);

        if (root_i!=root_j) {

            parent[root_i]=root_j;

        }

    }

};

void kruskal(int numVertices,vector<Edge>&edges)

{

    sort(edges.begin(), edges.end(), compareEdges);

  

    disjointset ds(numVertices);

    vector<Edge> mst; // 记录最终选中的电线方案

    int totalCost = 0;

    for (Edge&e :edges)

    {

        if(ds.find(e.u)!=ds.find(e.v))//判断是否有环

        {

            ds.unint(e.u, e.v);

            mst.push_back(e);

            totalCost+=e.weight;

            cout<<e.u<<"--"<<e.v<<"路径费用："<<e.weight<<endl;

        }

        else {

       cout << e.u << " -- " << e.v << " 已经成环，跳过！" << endl;

        }

    }

    cout<<"该路径花费"<<totalCost<<endl;

    cout<<"生成最小生成树"<<endl;;

    for (int i = 0; i < mst.size(); i++) {

        cout<<mst[i].u<<"--"<<mst[i].v<<endl;

    }

}

int main ()

{

    int vertices = 4; // 4 个城市

    vector<Edge> edges;

  

    // 输入所有的修路方案（报价单）

    // 0-1(1), 1-2(2), 0-2(3), 2-3(4), 0-3(5)

    edges.push_back({0, 1, 1});

    edges.push_back({1, 2, 2});

    edges.push_back({0, 2, 3});

    edges.push_back({2, 3, 4});

    edges.push_back({0, 3, 5});

  

    kruskal(vertices, edges);

  

    return 0;

}
```