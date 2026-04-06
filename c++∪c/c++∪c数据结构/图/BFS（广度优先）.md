
---

### 🎬 BFS 的“水波纹”动画

我们还是用这 4 个人的朋友圈（0, 1, 2, 3）。这次我们要从 **0 号**开始往外扩散：

1. **第一阶段（波纹中心）：**
    
    - 搜救队降落在 **0 号**，插上红旗（标记已访问）。
        
2. **第二阶段（第一圈波纹）：**
    
    - 0 号翻开抽屉，发现朋友有 `2` 和 `1`。
        
    - 搜救队**不急着深入**，而是先把 0 的所有直接朋友 `2` 和 `1` 都通知到，并给它们插上红旗。
        
3. **第三阶段（第二圈波纹）：**
    
    - 接下来，搜救队去探寻“朋友的朋友”。
        
    - 先看 `2` 的抽屉，发现朋友有 `3`, `1`, `0`。因为 `1` 和 `0` 都去过了，所以只把新朋友 `3` 标记并插上红旗。
        
    - 再看 `1` 的抽屉，发现朋友有 `2` 和 `0`，但都去过了，没有新发现。
        

**✨ 搜救结束！** 走过的顺序是：**0 ➡️ 2 ➡️ 1 ➡️ 3**（或者 0 ➡️ 1 ➡️ 2 ➡️ 3，取决于你先翻到谁）。

你看，它是**按距离由近及远**，一层一层剥开的！

---

### 🛠️ 核心武器：队列（Queue）

因为 BFS 是“先来的邻居先服务”，这太符合**排队**的规律了！所以我们需要用到**队列（Queue）**——先进先出（FIFO）。

- **BFS 的固定套路：**
    
    1. 把起点扔进队列，标记为已访问。
        
    2. 只要队列不为空，就从队头拉出一个人。
        
    3. 把这个人的**所有没访问过的邻居**，通通塞进队尾，并标记为已访问。
        
    4. 重复 2 和 3，直到队列空了，大功告成！
        

---

### 💻 怎么用 C++ 代码实现？

在 C++ 里，我们不需要像 C 语言那样手写一个复杂的队列结构体，标准库直接给我们提供了现成的 `std::queue`，非常方便！

我们在你之前那份完美的邻接表代码里，加入这段 BFS 代码。

#### 1. 引入队列头文件

在代码的最顶端加上：

C++

```
#include <queue> // 引入 C++ 的队列
```

#### 2. BFS 函数

C++

```
void BFS(Graph *graph, int startVertex) {
    // 1. 依然需要一个小红旗数组
    bool *visited = new bool[graph->numVertices];
    for (int i = 0; i < graph->numVertices; i++) {
        visited[i] = false;
    }

    // 2. 创建一个队列，里面装顶点的编号（int）
    queue<int> q;

    // 3. 把起点扔进队列，并插上红旗
    visited[startVertex] = true;
    q.push(startVertex);

    cout << "BFS 遍历结果（从顶点 " << startVertex << " 开始）: ";

    // 4. 只要队列里还有人，就继续搜救
    while (!q.empty()) {
        // 从队头拉出一个人
        int currentVertex = q.front();
        cout << currentVertex << " ";
        q.pop(); // 出队

        // 翻开这个人的抽屉，找邻居
        Node *temp = graph->adjlist[currentVertex];
        while (temp) {
            int neighbor = temp->vertex;

            // 如果邻居没去过，插上红旗，并塞到队尾排队
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
            temp = temp->next; // 找下一个邻居
        }
    }
    cout << endl;

    // 记得释放内存
    delete[] visited;
}
```

---

### 🛡️ 战术总结：DFS 与 BFS 的终极区别

|**特性**|**DFS (深度优先)**|**BFS (广度优先)**|
|---|---|---|
|**思想**|孤注一掷，一条路走到黑|稳扎稳打，层层推进|
|**核心数据结构**|**递归**（系统栈）|**队列**（Queue）|
|**最大用处**|走迷宫、找所有路径、拓扑排序|**寻找最短路径**（比如迷宫最短要几步）|

## 完整代码
```
#include<iostream>

#include<queue>

using namespace std;

typedef struct Node{

    int vertex;

    Node *next;

}Node;

  

typedef struct Graph{

    int numVertices;

    Node **adjlist;

}Graph;

Node *creatnode(int v)

{

    Node *newnode= new Node;

    newnode->vertex=v;

    newnode->next=nullptr;

    return newnode;

}

Graph *creatgraph(int vertices){

    Graph *graph=new Graph;

    graph->numVertices=vertices;

    graph->adjlist=new Node*[vertices];

    for(int i=0;i<vertices;i++){

        graph->adjlist[i]=nullptr;

    }

    return graph;

}

void addEdge(Graph *graph,int i,int j)

{

    Node *newnode=creatnode(j);

    newnode->next=graph->adjlist[i];

    graph->adjlist[i]=newnode;

    newnode=creatnode(i);

    newnode->next=graph->adjlist[j];

    graph->adjlist[j]=newnode;

}

void printgraph(Graph *grap) {

    for(int i=0;i<grap->numVertices;i++){

        Node *temp=grap->adjlist[i];

        cout<<"第"<<i<<"个顶点";

         while(temp){

            cout<<" "<<temp->vertex;

            temp=temp->next;

         }

         cout<<endl;

    }

}

void DFSexplore(Graph *graph,int v,bool visited[])

{

    visited[v]=true;

    cout<<v<<" ";

    Node *temp=graph->adjlist[v];

    while(temp){

        int neighbor=temp->vertex;

        if(!visited[neighbor]){

            DFSexplore(graph,neighbor,visited);

        }

        temp=temp->next;

    }

}

void DFS(Graph *graph,int startVertex)

{

    bool *visited=new bool[graph->numVertices];

    for(int i=0;i<graph->numVertices;i++){

        visited[i]=false;

    }

    cout << "DFS 遍历结果（从顶点 " << startVertex << " 开始）: ";

    DFSexplore(graph,startVertex,visited);

    cout<<endl;

    delete []visited;

}

  

void BFS(Graph *graph,int startVertex)

{

    bool *visited=new bool[graph->numVertices];

    for(int i=0;i<graph->numVertices;i++)

    {

        visited[i]=false;

    }

    queue<int>q;

    visited[startVertex]=true;

    q.push(startVertex);

    while(!q.empty())

    {

        int currentvertex=q.front();

        cout<<q.front()<<" ";

        q.pop();

        Node *temp=graph->adjlist[currentvertex];

        while(temp)

        {

            int neighbor=temp->vertex;

            if (!visited[neighbor]) {

            visited[neighbor]=true;

            q.push(neighbor);

            }

            temp=temp->next;

        }

}

}

int main ()

{

    Graph *graph=creatgraph(4);

    addEdge(graph, 0, 1);

    addEdge(graph, 0, 2);

    addEdge(graph, 1, 2);

    addEdge(graph, 2, 3);

    printgraph(graph);

    cout << "-------------------" << endl;

    DFS(graph,0);

    cout << "-------------------" << endl;

    BFS(graph,0);

    cout<<endl;

    return  0;

}
```