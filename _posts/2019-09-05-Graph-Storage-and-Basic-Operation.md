---
title: 5.2 Graph Storage and Basic Operation
---

图的存储必须要完成、准确地反映顶点集和边集的信息。根据不同图的结构和算法，可以采用不同的存储方式，但不同的存储方式将对程序的效率产生相当大的影响，因此所选的存储结构应适合于欲求解的问题。无论是无向图还是有向图，主要的存储方式有两种：邻接矩阵和邻接链表。前者属于图的顺序存储结构，后者属于图的链接存储结构。

### 1. 邻接矩阵法

用一个一维数组存储图中顶点的信息，用一个二维数组存储图中边的信息（即各顶点之间的邻接关系），存储顶点之间邻接关系的二维矩阵称为邻接矩阵。

普通图 ，`a[v][w]` 为 0 表示 v 和 w 没有连接，为 1 表示  `<v,w>` 或 `(v,w)` 。

带权图，`a[v][w]` 存储权值，0 或 ∞ 表示无连接关系，否则有连接关系 。

**注意**

1. 在简单应用中，可直接用二维数组作为图的邻接矩阵（顶点信息等均可省略）。
2. 当邻接矩阵中的元素仅表示相应的边是否存在时，边的类型可定义为布尔型，省空间
3. 邻接矩阵表示法的空间复杂度为 O(n^2) ，其中 n 为图的定点数 `|V|`

图的邻接矩阵存储表示法具有以下特点

1. 无向图的邻接矩阵一定是一个对称矩阵（并且唯一）。因此，在实际存储邻接矩阵时只需存储上（或下）三角矩阵的元素。

2. 对于无向图，邻接矩阵的第 i 行（或第 i 列）非零元素（或非 ∞ 元素）的个数正好是第 i 个顶点的度 TD(vi)

3. 对于有向图，邻接矩阵的第 i 行（或第 i 列）非零元素（或非 ∞ 元素）的个数正好是第 i 个顶点的出度 OD(vi) （或入度 ID(vi)）

4. 用邻接矩阵法存储图，很容易确定图中任意两个顶点之间是否有边相连。但是，要确定图中有多少条边，则必须按行、列对每个元素进行检测，所花费的时间代价很大。这是用邻接矩阵存储图的局限性。

5. 稠密图适合使用邻接矩阵的存储表示。

6. 设图 G 的邻接矩阵为 A ，A^n 的元素 `A^n[i][j]` 等于由顶点 i 到顶点 j 的长度为 n 的路径的数目。

   `Aij^2 = ΣAik·Akj` ，其中 k 为 1~n ，也就是说长度为 2 的路径最大为 n，最小为 0 。高阶的类推 。

## 2. 邻接表法

当一个图为稀疏图时，使用邻接矩阵表示法显然要浪费大量的存储空间，而图的邻接表法结合了顺序存储和链式存储方法，大大减少了这种不必要的浪费 。

所谓邻接表，是指对图 G 中的每个顶点 vi 建立一个单链表，第 i 个单链表中的结点表示依附于顶点 vi 的边（对于有向图则是以顶点 vi 为尾的弧），这个单链表就称为顶点 vi 的边表（对于有向图则称为出边表）。边表的头指针和顶点的数据采用顺序存储（称为顶点表），所以在邻接表中存在两种结点：顶点表结点和边表结点。

顺序表存储的结点中包含顶点数据和头指针，头指针指向的链表是邻接边链表，存储邻接边的编号 。

图的邻接表存储方法具有以下特点

1. 若 G 为无向图，则所需的存储空间为 `O(|v|+2|E|)` ；若 G 为有向图，则所需的存储空间为 `O(|V|+|E|)` 。前者的倍数 2 由于无向图中，每条边在邻表中出现了两次。
2. 对于稀疏图，采用邻接表表示将极大地节省存储空间。
3. 在邻接表中，给定一顶点，能很容易地找到它的所有邻边，因为只需要读取它的邻接表。在邻接矩阵中，相同的操作则需要扫描一行，花费的时间为 O(n) 。但是，若要确定给定两个顶点间是否存在边，则在邻接矩阵中可以立刻查到，而在邻接表中则需要在相应结点对应的边表中查找另一个结点，效率较低。
4. 图的邻接表表示并不唯一，因为在每个顶点对应的单链表中，各边结点的链接次序可以是任意的，它取决于建立链接表的算法及边的输入次序。

## 3. 十字链表

十字链表是有向图的一种链式存储结构。在十字链表中，对应于有向图中的每条弧有一个结点，对应于每个顶点也有一个结点。

弧结点中有 5 个域 ：尾域（tailvex）和头域（headvex）分别指示狐尾和弧头这两个顶点在图中的位置；链域 hlink 指向弧头相同的下一条弧；链域 tlink 指向弧尾相同的下一条弧；info 域指向该弧的相关信息。这样，弧头相同的弧就在同一个链表上，弧尾相同的弧也在同一个链表上。

顶点结点中有 3 个域 ：data 域存放顶点相关的数据信息，如顶点名称；firstin 和 firstout 两个域分别指向以该结点为弧头或弧尾的第一个弧结点。

注意，顶点结点之间是顺序存储的。

在十字链表中，即容易找到 vi 为尾的弧，又容易找到 vi 为头的弧，因而容易求得顶点 的出度和入度。

图的十字链表表示不是唯一的，但一个十字链表表示确定一个图。

### 4. 邻接多重表

邻接多重表是无向图的另一种链式存储结构。

在邻接表中，容易求得顶点和边的各种信息，但在邻接表中求两个顶点之间是否存在边而对边执行删除等操作时，需要分别在两个顶点的边表中遍历，效率较低。

与十字链表相似，在邻接多重表中，每条边用一个结点表示，结构如下

```c++
mark ivex ilink jvex jlink info
```

其中，mark 为标志域，可用以标记该条边是否被搜索过；ivex 和 jvex 为该边依附的两个顶点在图中的位置；ilink 指向下一条依附于顶点 ivex 的边；jlink 指向下一条依附于顶点 jvex 的边，info 为指向和边相关的各种信息的指针域。

每个顶点也用一个结点表示，其中包括 data 域和 fristedge 域（指示第一条依附于该顶点的边）

在邻接多重表中，所有依附于同一顶点的边串联在同一链表中，由于每条边依附于两个顶点，因此每个边结点同时链接在两个链表中。

### 5. 图的基本操作

图的基本操作是独立于图的存储结构的。而对于不同的存储方式，操作算法的具体实现会有着不同的性能。

图的基本操作主要包括（仅抽象地考虑，故忽略各变量的类型）

- `Adjacent(G, x, y)` ：判断图 G 是否存在边 `<x,y>` 或 `(x,y)` 
- `Neighbors(G, x)` ：列出图 G 中与结点 x 邻接的边
- `InsertVertex(G, x)` ：在图 G 中插入顶点 x
- `DeleteVertex(G, x)` ：从图 G 中删除顶点 x
- `AddEdge(G, x, y)` ：若无向边 `(x,y)` 或有向边 `<x,y>` 不存在，则向图 G 中添加该边
- `RemoveEdge(G,X,Y)` ：若无向边 `(x,y)` 或有向边 `<x,y>` 存在，则从图 G 中删除该边
- `FirstNeighbor(G,x)` ：求图 G 中顶点 x 的第一个邻接点，若有则返回顶点号。若 x 没有邻接点或图中不存在 x ，则返回 -1 。
- `NextNeighbor(G,x,y)` ：假设图 G 中顶点 y 是顶点 x 的一个邻接点，返回除 y 之外顶点 x 的下一个邻接点的顶点号，若 y 是 x 的最后一个邻接点，则返回 -1 。
- `GetEdgeValue(G,x,y)` ：获取图 G 中边 `(x,y)` 或 `<x,y>` 对应的权值 。
- `SetEdgeValue(G,x,y)` ：设置图 G 中边 `(x,y)` 或 `<x,y>` 对应的权值为 v。

此外，还有图的遍历算法 ：按照某种方式访问图中的每个顶点且仅访问一次。图的遍历算法包括深度优先遍历和广度优先遍历，这部分需要单独讲。