[广度优先搜索](https://blog.csdn.net/qq_41681241/article/details/81432634)

+ 具体思想：从图中某顶点a出发，在访问了a之后依次访问a的各个未曾访问过的邻接点，然后分别从这些邻接点出发依次访问它们的邻接点，并使得“先被访问的顶点的邻接点先于后被访问的顶点的邻接点被访问，直至图中所有已被访问的顶点的邻接点都被访问到。如果此时图中尚有顶点未被访问，则需要另选一个未曾被访问过的顶点作为新的起始点，重复上述过程，直至图中所有顶点都被访问到为止。

+ 用途：求最短路径或最优方案。

+ 特点：

+ 常用于找单一的最短路线，或者是规模小的路径搜索，它的特点是”搜到就是最优解”。

+ 像搜索最短路径这些的很显著是用广搜，因为广搜的特征就是一层一层往下搜的，保证当前搜到的都是最优解，当然，最短路径只是一方面的操作，像什么起码状态转换也是可以操作的。

+ 广搜则是操作了队列，边进队，边出队。

不管是BFS还是DFS，它们虽然好用，但由于时间和空间的局限性，以至于它们只能解决数据量小的问题。

----



**迷宫问题**，求左上角到右下角的最短路径。

**二叉树的层次遍历**也属于广搜。