---
title: 图论
date: 2019-08-28 17:46:07
tags: 
- 数据结构与算法
- 学习
categories: 数据结构与算法分析
---

## 简述

​	图是一种**多对多**形式的数据结构，线性表（一对一）、树（一对多）都可以看作特殊形式的表。



## 术语

* **顶点**(Vertex)，顶点的集合一般用**V**表示

* **边**(Edge)，边的集合一般用**E**表示

  * ( v, w ) ∈ E，v、w ∈ V，这种边表示 **无向边**
  * &lt; v, w &gt; ∈ E， v、w ∈ V，这种边表示 **有向边**
  * 一般不考虑 **重边** 和 **自回路**

* **无向图**：所有边都是无向边的图

* **有向图**：所有边都是有向边的图

* **网络**：带权重的图

* **邻接点**：与一个顶点直接相连的点

* **度**：与顶点连接的边的条数，**有向图**中又存在 **入度** 和 **出度**

* > **连通图**：在[图论](https://baike.baidu.com/item/图论/1433806)中，连通图基于连通的概念。在一个[无向图](https://baike.baidu.com/item/无向图/1680427) G 中，若从[顶点](https://baike.baidu.com/item/顶点/11030118)i到顶点j有路径相连（当然从j到i也一定有路径），则称i和j是连通的。如果 G 是[有向图](https://baike.baidu.com/item/有向图)，那么连接i和j的路径中所有的边都必须同向。如果图中任意两点都是连通的，那么图被称作连通图。如果此图是有向图，则称为**强连通图**（注意：需要双向都有路径）。图的[连通性](https://baike.baidu.com/item/连通性/6688865)是图的基本性质。

* **连通分量**：无向图的极大连通子图



***

## 图的操作

### 图的创建

1. 邻接矩阵：

   1. 邻接矩阵是一个表示顶点相邻关系的矩阵，若两个矩阵相邻，则矩阵的对应位置值为1（若边有权重，则表示权重），否则为0

   ![图片来自百度百科](https://gss1.bdstatic.com/-vo3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike80%2C5%2C5%2C80%2C26/sign=77989746352ac65c73086e219a9bd974/b812c8fcc3cec3fdb11cec53d688d43f879427f1.jpg)
   2. 在C语言中，可用G[ N ] [ N ]来表示邻接矩阵
   3. 邻接矩阵的对角线为0，且无向图的邻接矩阵关于对角线对称
   4. 无向图的邻接矩阵可以用长度为$N(N+1)/2$的数组表示，G~ij~在该数组中的下标为$( i*(i+1)/2 + j ) $
   5. 邻接矩阵的第 i 行的数组元素值为1的个数表示顶点i的出度，第i列的数组元素值为1表示顶点 i 的入度
   6. 优点
      * 简单、直观、好理解
      * 方便检查任意一对顶点之间是否存在边
      * 方便找一个顶点的所有邻接点
      * 方便计算任一顶点的度
   7. 缺点：
      * 存储稀疏图时，浪费空间
      * 浪费时间（如统计稀疏图中一共有多少边）

2. 邻接表：G[ N ]为一个指针数组，对应矩阵每行一个链表，只保存等于1的数据

   ![图片来自网络](http://www.ahalei.com/data/attachment/forum/201404/08/091650gyll6hbqbjyxls8s.png)

   * 适合稀疏图使用



邻接矩阵图的创建

```cpp
/* 图的邻接矩阵表示法 */
 
#define MaxVertexNum 100    /* 最大顶点数设为100 */
#define INFINITY 65535        /* ∞设为双字节无符号整数的最大值65535*/
typedef int Vertex;         /* 用顶点下标表示顶点,为整型 */
typedef int WeightType;        /* 边的权值设为整型 */
typedef char DataType;        /* 顶点存储的数据类型设为字符型 */
 
/* 边的定义 */
typedef struct ENode *PtrToENode;
struct ENode{
    Vertex V1, V2;      /* 有向边<V1, V2> */
    WeightType Weight;  /* 权重 */
};
typedef PtrToENode Edge;
        
/* 图结点的定义 */
typedef struct GNode *PtrToGNode;
struct GNode{
    int Nv;  /* 顶点数 */
    int Ne;  /* 边数   */
    WeightType G[MaxVertexNum][MaxVertexNum]; /* 邻接矩阵 */
    DataType Data[MaxVertexNum];      /* 存顶点的数据 */
    /* 注意：很多情况下，顶点无数据，此时Data[]可以不用出现 */
};
typedef PtrToGNode MGraph; /* 以邻接矩阵存储的图类型 */
 
 
 
MGraph CreateGraph( int VertexNum )
{ /* 初始化一个有VertexNum个顶点但没有边的图 */
    Vertex V, W;
    MGraph Graph;
     
    Graph = (MGraph)malloc(sizeof(struct GNode)); /* 建立图 */
    Graph->Nv = VertexNum;
    Graph->Ne = 0;
    /* 初始化邻接矩阵 */
    /* 注意：这里默认顶点编号从0开始，到(Graph->Nv - 1) */
    for (V=0; V<Graph->Nv; V++)
        for (W=0; W<Graph->Nv; W++)  
            Graph->G[V][W] = INFINITY;
             
    return Graph; 
}
        
void InsertEdge( MGraph Graph, Edge E )
{
     /* 插入边 <V1, V2> */
     Graph->G[E->V1][E->V2] = E->Weight;    
     /* 若是无向图，还要插入边<V2, V1> */
     Graph->G[E->V2][E->V1] = E->Weight;
}
 
MGraph BuildGraph()
{
    MGraph Graph;
    Edge E;
    Vertex V;
    int Nv, i;
     
    scanf("%d", &Nv);   /* 读入顶点个数 */
    Graph = CreateGraph(Nv); /* 初始化有Nv个顶点但没有边的图 */ 
     
    scanf("%d", &(Graph->Ne));   /* 读入边数 */
    if ( Graph->Ne != 0 ) { /* 如果有边 */ 
        E = (Edge)malloc(sizeof(struct ENode)); /* 建立边结点 */ 
        /* 读入边，格式为"起点 终点 权重"，插入邻接矩阵 */
        for (i=0; i<Graph->Ne; i++) {
            scanf("%d %d %d", &E->V1, &E->V2, &E->Weight); 
            /* 注意：如果权重不是整型，Weight的读入格式要改 */
            InsertEdge( Graph, E );
        }
    } 
 
    /* 如果顶点有数据的话，读入数据 */
    for (V=0; V<Graph->Nv; V++) 
        scanf(" %c", &(Graph->Data[V]));
 
    return Graph;
}
```



邻接表图的创建

```cpp
/* 图的邻接表表示法 */
 
#define MaxVertexNum 100    /* 最大顶点数设为100 */
typedef int Vertex;         /* 用顶点下标表示顶点,为整型 */
typedef int WeightType;        /* 边的权值设为整型 */
typedef char DataType;        /* 顶点存储的数据类型设为字符型 */
 
/* 边的定义 */
typedef struct ENode *PtrToENode;
struct ENode{
    Vertex V1, V2;      /* 有向边<V1, V2> */
    WeightType Weight;  /* 权重 */
};
typedef PtrToENode Edge;
 
/* 邻接点的定义 */
typedef struct AdjVNode *PtrToAdjVNode; 
struct AdjVNode{
    Vertex AdjV;        /* 邻接点下标 */
    WeightType Weight;  /* 边权重 */
    PtrToAdjVNode Next;    /* 指向下一个邻接点的指针 */
};
 
/* 顶点表头结点的定义 */
typedef struct Vnode{
    PtrToAdjVNode FirstEdge;/* 边表头指针 */
    DataType Data;            /* 存顶点的数据 */
    /* 注意：很多情况下，顶点无数据，此时Data可以不用出现 */
} AdjList[MaxVertexNum];    /* AdjList是邻接表类型 */
 
/* 图结点的定义 */
typedef struct GNode *PtrToGNode;
struct GNode{  
    int Nv;     /* 顶点数 */
    int Ne;     /* 边数   */
    AdjList G;  /* 邻接表 */
};
typedef PtrToGNode LGraph; /* 以邻接表方式存储的图类型 */
 
 
 
LGraph CreateGraph( int VertexNum )
{ /* 初始化一个有VertexNum个顶点但没有边的图 */
    Vertex V;
    LGraph Graph;
     
    Graph = (LGraph)malloc( sizeof(struct GNode) ); /* 建立图 */
    Graph->Nv = VertexNum;
    Graph->Ne = 0;
    /* 初始化邻接表头指针 */
    /* 注意：这里默认顶点编号从0开始，到(Graph->Nv - 1) */
       for (V=0; V<Graph->Nv; V++)
        Graph->G[V].FirstEdge = NULL;
             
    return Graph; 
}
        
void InsertEdge( LGraph Graph, Edge E )
{
    PtrToAdjVNode NewNode;
     
    /* 插入边 <V1, V2> */
    /* 为V2建立新的邻接点 */
    NewNode = (PtrToAdjVNode)malloc(sizeof(struct AdjVNode));
    NewNode->AdjV = E->V2;
    NewNode->Weight = E->Weight;
    /* 将V2插入V1的表头 */
    NewNode->Next = Graph->G[E->V1].FirstEdge;
    Graph->G[E->V1].FirstEdge = NewNode;
         
    /* 若是无向图，还要插入边 <V2, V1> */
    /* 为V1建立新的邻接点 */
    NewNode = (PtrToAdjVNode)malloc(sizeof(struct AdjVNode));
    NewNode->AdjV = E->V1;
    NewNode->Weight = E->Weight;
    /* 将V1插入V2的表头 */
    NewNode->Next = Graph->G[E->V2].FirstEdge;
    Graph->G[E->V2].FirstEdge = NewNode;
}
 
LGraph BuildGraph()
{
    LGraph Graph;
    Edge E;
    Vertex V;
    int Nv, i;
     
    scanf("%d", &Nv);   /* 读入顶点个数 */
    Graph = CreateGraph(Nv); /* 初始化有Nv个顶点但没有边的图 */ 
     
    scanf("%d", &(Graph->Ne));   /* 读入边数 */
    if ( Graph->Ne != 0 ) { /* 如果有边 */ 
        E = (Edge)malloc( sizeof(struct ENode) ); /* 建立边结点 */ 
        /* 读入边，格式为"起点 终点 权重"，插入邻接矩阵 */
        for (i=0; i<Graph->Ne; i++) {
            scanf("%d %d %d", &E->V1, &E->V2, &E->Weight); 
            /* 注意：如果权重不是整型，Weight的读入格式要改 */
            InsertEdge( Graph, E );
        }
    } 
 
    /* 如果顶点有数据的话，读入数据 */
    for (V=0; V<Graph->Nv; V++) 
        scanf(" %c", &(Graph->G[V].Data));
 
    return Graph;
}
```

### 图的遍历

1. **深度优先搜索**（Depth First Search，**DFS**）

   1. 深度优先沿着路径走知道不能深入为止，然后退回到某一个顶点，在选择该顶点的其他路径

   2. 由于深度优先搜索是不断深入然后退回，故其实现过程可以用**堆栈**来描述，所以深度优先搜索也可以用递归来实现

      *伪代码如下*

      ```c
      void DFS(Vertex V)
      {
          visited[V] = true;
          for(V的每个邻接点W)
              if(!visited[W])
                  DFS(W);
      }
      ```

      

2. **广度优先搜索**（Breadth First Search，**BFS**）

   1. 搜索一个顶点相邻的所有节点，随后继续访问相邻节点的未访问相邻节点

   2. 可以用**队列**来实现

      *伪代码如下*

      ```c
      void BFS(Vertex V)
      {
          visited[V] = true;
          Enqueue(V, Q);
          while(!IsEmpty(Q)){
              V = Dequeue(Q);
              for(V的每个邻接点W){
                  visited[w]= true;
                  Enqueue(W, Q);
              }
          }
      }
      ```



**注**：每调用一次DFS或BFS，就把V所在的连通分量遍历了一遍，故对于不连通图，只需要采取以下程序来进行遍历

```c
void ListComponents(Graph G)
{
    for(each V in G)
        if(!visited[V]){
            DFS[V];
        }
}
```



## 最短路径问题

* 分类：
  * 单源最短路径问题
    * （有向）无权图
    * （有向）有权图
  * 多源最短路径问题

### 无权图的单源最短路算法

* 按照递增（非递减）的顺序找出到各个顶点的最短路，与BFS类似，但略有不同

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190829162001.png)

* *伪代码如下*

  ```c
  //dist[W]：S到W的最短距离，dist[s]=0, 其余可以初始化为-1、正无穷、负无穷等
  //path[W]：S到W的路上会经过的某点
  void Unweighted(Vertex S)
  {
      Enqueue(S,Q);
      while(!IsEmpty(Q)){
          V = Dequeue(Q);
          for(V的每个邻接点W){
              if(dist[W] = -1){
                  dist[W] = dist[V] + 1;
                  path[W] = V; //记录了该顶点在最短路径上的上一个点
                  Enqueue(W, Q);
              }
          }
      }
  }
  //O(V+E)
  ```



### 有权图的单源最短路径算法

![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190829163613.png)

<center>负值圈的情况一般不进行讨论</center>
* 思路：按照递增的顺序找出到各个顶点的最短路

* Dijkstra算法：

  * 令S={ 源点s + 已经确定了最短路径的顶点v~i~ }

  * 对任一未收录的顶点v，定义dist[v]为s到v的最短路径长度，但该路径仅经过S中的顶点。即路径{ s→(v~i~∈S)→v }的最小长度

  * 若路径是按照递增（非递减）的顺序生成的，则

    * 真正的最短路必须只经过S中的顶点
    * 每次从未收录的定点中选一个dist最小的收录(贪心思想)
    * 增加一个v进入S，可能影响另一个w的dist值(如果v的加入使得w的dist值变小，则v一定在s到w的最短路径上，且v和w必有一条边相连)
      * dist[w]  = min{dist[w], dist[v] + <v, w>的权重}

    *伪代码如下*<a name="Dijkstra" href=""></a>

    ```c
    //dist[s]可初始化为0，其相邻点可初始化为它们之间边的权重，其余则初始化为正无穷
    void Dijkstra(Vertex s)
    {
        while(1)
        {
            V = 未收录定点中dist最小者; //（1）
            if(这样的V不存在)
                break;
            
            collected[V] = true;
                for(V的每个邻接点W)
                    if(collected[W] == false)
                        if(dist[V] + E<V,W> < dist[W]){
                            dist[W] = dist[V] + E<V,W>; //（2）
                            path[W] = V;
                        }
        }
    }
    //不能解决有负边的情况
    //该算法复杂度取决于（1）（2）步骤，
    ```

  * 复杂度不同的解决方法：

    ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190829165824.png)



### 多源最短路径算法

* 方法1：直接将单源最短路径调用|V|次
  * T = O(|V|^3^+|E|×|V|)，对于稀疏图的效果号
* 方法2：Floyd算法
  * T = O(|V|^3^)，对稠密图效果好





* Floyd算法

  * D^k^[ i ][ j ] = 路径{i → { l ≤ k(编号小于等于k的顶点)} → j}的最小长度
  * D^0^，D^1^，D^2^ ，D^3^，D^|v|-1^[ i ][ j ]即给出了 i 到 j 的正整最短距离
  * D^-1^应该定义为带权的邻接矩阵，对角元为0；如果 i 和 j 之间没有直接关系，应该将D^-1^对应的值初始化为正无穷
  * 当D^k-1^已经完成，递推到D^k^时：
    * k ∉ 最短路径{i → { l ≤ k} → j}，则D^k^ = D^k-1^
    * k ∈ 最短路径{i → { l ≤ k} → j}，则D^k^[ i ][ j ]  = D^k-1^[ i ][ k ]  +  D^k-1^[ k][ j ]

  ```c
  void Floyd()
  {
      for(i = 0; i < N; i++)
          for(j = 0; j < N; j++)
          {
              D[i][j] = G[i][j];
              path[i][j] = -1;	//用于记录路径
          }
      for(k = 0; k < N; k++)
          for(i = 0; i < N; i++)
              for(j = 0; j < N; j++)
                  if(D[i][k] + D[k][j] > D[i][j]){
                      D[i][j] = D[i][k] + D[k][j];
                      path[i][j] = k;
                  }
  }
  ```

  

## 最小生成树问题

* **最小生成树（Mininum Spanning Tree）**:

  * 是一棵**树**：
    * 无回路
    * |V|个顶点一定有|V-1|条边
  * 是**生成树**：向生成树中任意加一条边都会构成回路
    * 包含全部顶点
    * |V|-1条边都在图里
  * 边的权重**和**最小

  ![](https://wnghilin-blog.oss-cn-beijing.aliyuncs.com/20190830173241.png)

  

* 思想：有约束的**贪心算法**
  * 约束：
    - 只能用图里有的边
    - 只能正好用掉|V|-1条边
    - 不能有回路



### Prim算法——让一棵小树长大

* 类似Dijkstra算法：<a href="#Dijkstra">Dijkstra伪代码码</a>
* dist[V] = E(s, V) 或正无穷
* parent[s] = -1
* *Prim算法的伪代码*

```c
void Prim()
{
    MST = {s};
    while(1)
    {
        V = 未收录定点中dist最小者;
        if(这样的V不存在)
            break;
        将V收录进MST中;
        for( V的每个邻接点W )
            if( dist[W] != 0//W点未被收录 )
                if( E(V,W) < dist[W] )
                {
                    dist[W] = E(V,W);
                    parent[W] = V;
                }
    }
    if(MST中收到的点不到|V|个)
               Error("生成树不存在");
}
```

<center>适合稠密图</center>



### Kruskal算法——将森林合并成树

* 直接按**递增**顺序选出图中权重最小的边（但不能与已选中的边构成回路）
* 伪代码如下：

```c
void Kruskal(Graph G)
{
    MST = { };
    while(MST中步到|V|-1条边 && E中还有边)
    {
        从E中取一条权重最小的边E(V,W); /*最小堆*/
        将E(V,W)从E中删除;
        if(E(V,W)不在MST中构成回路)	/*并查集*/
            将E(V,W)加入MST;
        else
            彻底无视E(V,W);
    }
    if(MST中不到|V|-1条边)
        Error("生成树不存在");
}
```



