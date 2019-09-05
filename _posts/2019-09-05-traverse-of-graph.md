---
title: 5.3 traverse of graph
---

图的遍历是指从图中的某一顶点出发，按照某种搜索方法沿着图中的边对图中的所有顶点访问一次且仅访问一次。注意到树是一种特殊的图，所以树的遍历实际上也可视为一种特殊的图的遍历。图的遍历是图的一种最基本的操作，其他操作都建立在图的遍历操作基础之上。

图的遍历主要有两种算法：广度优先搜索和深度优先搜索。包括广度优先搜索和深度优先搜索在内的几乎所有图的搜索算法，都可以抽象为优先级搜索或最佳优先搜索。广度优先搜索会优先考虑最早被发现的顶点，也就是说离起点越近的顶点其优先级越高。深度优先搜索会优化考虑最后被发现的顶点。广度优先搜索算法在研究迷宫路径问题时被发现；深度优先搜索在人工智能方面得广泛应用。

## 1. 广度优先搜索
