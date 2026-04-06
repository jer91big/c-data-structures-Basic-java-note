### 🎬 Dijkstra 算法的“动态铺路”动画

假设我们要从 **起点 A** 去往 **终点 D**。

地图上还有 B 和 C 两个中转站，它们之间的距离（权重）如下：

- A 到 B 距离是 **2**
    
- A 到 C 距离是 **6**
    
- B 到 C 距离是 **1**
    
- B 到 D 距离是 **5**
    
- C 到 D 距离是 **1**
    

探险开始！Dijkstra 算法最神奇的地方在于，它在脑子里有一本**“账本”**，时刻记录着：**从起点 A 到各个城市，当前已知的最短距离是多少。**

#### 📍 第一幕：初始状态（全都是未知数）

- 账本上写着：
    
    - A ➡️ A：距离为 **0**（自己到自己）。
        
    - A ➡️ B：未知（记为 $\infty$ 无穷大）。
        
    - A ➡️ C：未知（记为 $\infty$ 无穷大）。
        
    - A ➡️ D：未知（记为 $\infty$ 无穷大）。
        
- 探险队目前站在 **A**。
    

#### 📍 第二幕：从 A 出发，探查邻居

- 探险队在 A 发现，可以去 B，也可以去 C。
    
    - 去 B 距离是 2。账本更新：A ➡️ B 变成 **2**。
        
    - 去 C 距离是 6。账本更新：A ➡️ C 变成 **6**。
        
- **贪心选择：** 看看账本上除了 A，谁的距离最短？是 B（距离为 2）。
    
- 于是，探险队**空降到 B**，并宣布：从 A 到 B 的最短路径死锁为 2，以后不改了！
    

#### 📍 第三幕：从 B 出发，发现“抄近道”魔法！

- 现在探险队站在 B，翻开 B 的抽屉，发现可以去 C，也可以去 D。
    
    - 如果去 D：距离是 $2 + 5 = 7$。账本更新：A ➡️ D 变成 **7**。
        
    - 如果去 C：距离是 $2 + 1 = 3$。
        
- **高能时刻：** 探险队看了一眼账本，上面原本写着 A ➡️ C 的距离是 **6**（那是从 A 直达 C 的路）。
    
- 探险队大喊：“不对！从 A 先到 B，再从 B 到 C，总距离才 **3**！比直达还近！”
    
- 于是，账本发生**“松弛（Relaxation）”**：A ➡️ C 的距离从 6 涂改成 **3**。
    

#### 📍 第四幕：从 C 出发，直达终点

- 此时账本上未确定的城市里，C 的距离是 3（最短）。探险队**空降到 C**。
    
- 翻开 C 的抽屉，发现可以去 D。
    
    - 如果去 D：距离是 $3 + 1 = 4$。
        
- 探险队再看一眼账本：之前记录的 A ➡️ D 是 **7**（走 B ➡️ D 那条线）。
    
- “又可以抄近道！” 探险队再次涂改账本：A ➡️ D 的距离从 7 变成 **4**。
    

#### ✨ 终局：所有城市探索完毕！

最终账本死锁，我们得到了从 A 出发到所有点的绝对最短路径：

- A ➡️ B：**2**
    
- A ➡️ C：**3**（走 A ➡️ B ➡️ C）
    
- A ➡️ D：**4**（走 A ➡️ B ➡️ C ➡️ D）
    

---

### 🛡️ 算法的核心内功（三大步骤）

总结一下，Dijkstra 每次循环都在做三件事：

1. **选最短**：在账本里，找一个**目前距离最短**且**还没被锁死**的城市，走过去。
    
2. **锁死它**：宣布这个城市的最终最短距离已找到，插上红旗。
    
3. **改账本（松弛）**：站在这个城市，看看通过它能不能让邻居们的路途变短。如果能，就改写账本！
## 完整代码
```
#include <functional>
#include<iostream>
#include<vector>
#include<queue>
using namespace std;
const int INF=1e9;

typedef struct Node
{
    int vertex;
    int weight;
    Node *next;
}Node;
typedef struct Graph
{
    int numvertexNum;
    Node **adjacencyList;
}Graph;
Graph *creatgraph(int vertexNum)
{
    Graph *graph=new Graph;
    graph->numvertexNum=vertexNum;
    graph->adjacencyList=new Node*[vertexNum];
    for(int i=0;i<vertexNum;i++)
    {
        graph->adjacencyList[i]=NULL;
    }
    return graph;
}
Node *creatnode(int v,int w)
{
    Node *node=new Node;
    node->vertex=v;
    node->weight=w;
    node->next=nullptr;
    return node;
}
void addEdge(Graph *graph,int u,int v,int w)
{
    Node *node=creatnode(v,w);
    node->next=graph->adjacencyList[u];
    graph->adjacencyList[u]=node;
}
void dijkstra(Graph *graph,int startvertex)
{
    int n=graph->numvertexNum;
    vector<int>dist(n,INF);
    vector<bool>visited(n,false);
    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>>pq;
    dist[startvertex]=0;
    pq.push({0,startvertex});
    while (!pq.empty()) {
       int u=pq.top().second;
       pq.pop();
       if (visited[u])  continue;
        visited[u]=true;

        Node *temp=graph->adjacencyList[u];
        while (temp) {

            int v=temp->vertex;
            int w=temp->weight;

            if(!visited[v]&&dist[u]+w<dist[v])
            {
                dist[v]=dist[u]+w;
                pq.push({dist[v],v});

            }


            temp=temp->next;

        }
    
}
    for (int i=0;i<graph->numvertexNum;i++) {
        cout<<dist[i]<<" ";

    }
}
void deleteGraph(Graph *graph) {
    for (int i = 0; i < graph->numvertexNum; i++) {
        Node *temp = graph->adjacencyList[i];
        while (temp) {
            Node *next = temp->next;
            delete temp;
            temp = next;
        }
    }
    delete[] graph->adjacencyList;
    delete graph;
}
int main()
{
    Graph *graph=creatgraph(5);
    addEdge(graph,0,1,4);
    addEdge(graph,0,2,1);
    addEdge(graph,1,2,2);
    addEdge(graph,1,3,3);
    addEdge(graph,2,3,1);
    addEdge(graph,2,4,2);
    addEdge(graph,3,4,3);
    dijkstra(graph,0);

    deleteGraph(graph);



    return 0;
}
```