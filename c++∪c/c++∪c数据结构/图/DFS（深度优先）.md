在“图”的世界里，DFS 就像是一个**走迷宫的探险家**。

- **它的策略是：** 只要前面有没走过的路，就一头扎进去，绝不回头！直到走到死胡同，才退回到上一个路口，换条路继续扎进去。
    
- **它的核心武器：** 依然是你之前学过的**递归**（或者栈）。
    

---

## 🎬 DFS 的“探险家”动画

我们还是用你代码里的这 4 个人的朋友圈（0, 1, 2, 3）。现在我们要从 **0 号**开始，去探寻所有人：

1. **第一步：** 探险家降落在 **0 号**。
    
    - 插上一面小红旗（标记 0 已访问），免得待会儿转圈圈。
        
2. **第二步：** 0 号翻开抽屉，发现朋友有 `2` 和 `1`。
    
    - 探险家是个急性子，看到 `2`，**二话不说直接空降到 2 号**！
        
3. **第三步：** 探险家来到了 **2 号**。
    
    - 插上小红旗（标记 2 已访问）。
        
    - 2 号翻开抽屉，朋友有 `3`, `1`, `0`。
        
    - 看到 `3`，没去过！**直接空降到 3 号**。
        
4. **第四步：** 探险家来到了 **3 号**。
    
    - 插上小红旗（标记 3 已访问）。
        
    - 3 号翻开抽屉，只有朋友 `2`。但 2 号已经插过旗了（去过了）。
        
    - **死胡同！** 探险家原路退回到 2 号。
        
5. **第五步：** 回到 **2 号**。
    
    - 2 号抽屉里下一个是 `1`。1 没去过！**直接空降到 1 号**。
        
6. **第六步：** 探险家来到了 **1 号**。
    
    - 插上小红旗（标记 1 已访问）。
        
    - 1 号的朋友是 `2` 和 `0`，但这两个地方都去过了。
        
    - **死胡同！** 探险家退回到 2 号，再退回到 0 号。
        

**✨ 探险结束！** 探险家走过的顺序是：**0 ➡️ 2 ➡️ 3 ➡️ 1**。

---

## 💻 怎么用 C++ 代码实现？

为了不让探险家在朋友圈里“鬼打墙”（比如 0 找 1，1 又找 0，无限循环），我们需要一个布尔数组 `visited` 来记录谁插了小红旗。

我们在你那份完美的邻接表代码基础上，加上这两个函数：

#### 1. 递归探险兵（核心 DFS）

C++

```
void DFS_Explore(Graph *graph, int v, bool visited[]) {
    // 1. 到达新顶点，插上小红旗
    visited[v] = true;
    cout << v << " "; // 打印出探险家走过的足迹

    // 2. 翻开这个顶点的抽屉（遍历链表）
    Node *temp = graph->adjlist[v];
    while (temp != nullptr) {
        int neighbor = temp->vertex;
        
        // 3. 如果这个邻居没去过，直接“递归”空降过去！
        if (!visited[neighbor]) {
            DFS_Explore(graph, neighbor, visited);
        }
        temp = temp->next; // 找下一个邻居
    }
}
```

#### 2. 探险启动器（包装一下，方便调用）

C++

```
void DFS(Graph *graph, int startVertex) {
    // 动态创建一个数组，记录谁被访问过
    bool *visited = new bool[graph->numVertices];
    for (int i = 0; i < graph->numVertices; i++) {
        visited[i] = false; // 一开始大家都没被访问
    }

    cout << "DFS 遍历结果（从顶点 " << startVertex << " 开始）: ";
    
    // 吹响号角，开始探险！
    DFS_Explore(graph, startVertex, visited);
    
    cout << endl;
    delete[] visited; // 用完记得释放内存
}
```

---

## 🚀 终极组装

你只需要把你上一轮代码的 `main` 函数改成这样，就能看到 DFS 的威力了：

C++

```
int main() {
    Graph *graph = creatgraph(4);
    
    addEdge(graph, 0, 1);
    addEdge(graph, 0, 2);
    addEdge(graph, 1, 2);
    addEdge(graph, 2, 3);
    
    printgraph(graph); // 先看看图
    
    cout << "-------------------" << endl;
    
    DFS(graph, 0); // 从 0 号开始 DFS 走迷宫
    
    return 0;
}
```

这段代码完美的契合了你之前学的**树的递归思想**，因为图的 DFS 探险，本质上就是在图里强行扒拉出一棵“生成树”来遍历！

## 完整代码 
```
#include<iostream>
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
    
	return  0;
}
```