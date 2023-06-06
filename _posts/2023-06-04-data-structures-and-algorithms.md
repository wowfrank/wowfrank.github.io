---
layout: post
title: "Data Structures and Algorithms"
date: "2023-06-04 01:09:00 +0800"
description: "Data Structures and Algorithms" # (optional)
img: "2023-06-04-cover-image-data-structures-and-algorithms.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Data Structures and Algorithms', 'DSA', 'Algorithm']
categories: ['DSA']
---

## What are Data Structures?

Data structure is a storage that is used to store and organize data. It is a way of arranging data on a computer so that it can be accessed and updated efficiently.

## Types of Data Structure

- Linear data structure
    1. Array Data Structure
    2. Stack Data Structure: LIFO (**Last In First Out**) principle
    3. Queue Data Structure: FIFO (**First In First Out**) principle
        - Simple Queue
        - Circular Queue
        - Priority Queue
        - Double Ended Queue (Deque)
    4. Linked List Data Structure
- Non-linear data structure
    1. Graph Data Structure

        More precisely, a graph is a data structure (V, E) that consists of

        - A collection of vertices V
        - A collection of edges E, represented as ordered pairs of vertices (u,v)
        
        ![Vertices and Edges]({{site.baseurl}}/assets/img/2023-06-04/graph-vertices-edges_0.webp)

        ```
        In the graph:

        V = {0, 1, 2, 3}
        E = {(0,1), (0,2), (0,3), (1,2)}
        G = {V, E}
        ```

        ### Graph Representation

        1. Adjacency Matrix: is a 2D array of V x V vertices

        ![Adjacency Matrix]({{site.baseurl}}/assets/img/2023-06-04/adjacency-matrix_1.webp)

        2. Adjacency List: an array of linked lists

        ![Adjacency List]({{site.baseurl}}/assets/img/2023-06-04/adjacency-list.webp)


        Types:
        - Spanning Tree and Minimum Spanning Tree

        The total number of spanning trees with n vertices that can be created from a complete graph is equal to n^(n-2). The edges may or may not have **weights** assigned to them.
        
        A minimum spanning tree is a spanning tree in which the sum of the weight of the edges is as minimum as possible.

        - Strongly Connected Components

        A strongly connected component is the portion of a directed graph in which there is a path from each vertex to another vertex. It is applicable only on a directed graph.

        ![Strongly Connected Components]({{site.baseurl}}/assets/img/2023-06-04/scc-strongly-connected-components.webp)

        These components can be found using **Kosaraju's Algorithm**.



        - Adjacency Matrix
        - Adjacency List
    2. Trees Data Structure
        - Binary Tree
            1. Full Binary Tree: A full Binary tree is a special type of binary tree in which every parent node/internal node has either two or no children.
            1. Perfect Binary Tree: A perfect binary tree is a type of binary tree in which every internal node has exactly two child nodes and all the leaf nodes are at the same level.
            1. Complete Binary Tree: A complete binary tree is just like a full binary tree, but with two major differences
            1. Degenerate or Pathological Binary Tree: A degenerate or pathological tree is the tree having a single child either left or right.
            1. Skewed Binary Tree: A skewed binary tree is a pathological/degenerate tree in which the tree is either dominated by the left nodes or the right nodes. 
            1. Balanced Binary Tree: It is a type of binary tree in which the difference between the height of the left and the right subtree for each node is either 0 or 1.
        - Binary Search Tree
            
            The properties that separate a binary search tree from a regular binary tree is:

            1. All nodes of left subtree are less than the root node
            1. All nodes of right subtree are more than the root node
            1. Both subtrees of each node are also BSTs i.e. they have the above two properties

            _In the graph below, The binary tree on the right isn't a binary search tree because the right subtree of the node "3" contains a value smaller than it._

            ![Binary Search Tree]({{site.baseurl}}/assets/img/2023-06-04/bst-vs-not-bst.webp)

        - AVL Tree
            
            AVL tree is a self-balancing **binary search tree** in which each node maintains extra information called a **balance factor** whose value is either -1, 0 or +1.
            
            Balance Factor = (Height of Left Subtree - Height of Right Subtree) or (Height of Right Subtree - Height of Left Subtree)

            AVL tree got its name after its inventor Georgy Adelson-Velsky and Landis.

            An example of a balanced avl tree is

            ![AVL Tree]({{site.baseurl}}/assets/img/2023-06-04/avl-tree-final-tree-1_0_2.webp)

            Left-Right and Right-Left Rotate:

            ![AVL Tree Left Rotation]({{site.baseurl}}/assets/img/2023-06-04/avl-tree-leftright-rotate-1.png)

            ![AVL Tree Right Rotation]({{site.baseurl}}/assets/img/2023-06-04/avl-tree-leftright-rotate-2.png)

        - B-Tree
            
            B-tree is a special type of self-balancing search tree in which each node can contain more than one key and can have more than two children. It is also known as a height-balanced m-way tree.

            ![B Tree]({{site.baseurl}}/assets/img/2023-06-04/b-tree.webp)

            The **order** of the tree represents the maximum number of children a tree’s node could have. So when we say we have a B-Tree of order N, It means every node of that B-Tree can have a maximum of N children. For example, _a Binary Search Tree is a tree of **order 2** since each node has at most 2 children.

            The **degree** of a tree represents the maximum degree of a node in the tree. Recall for a given node, its degree is equal to the number of its children.

            **Degree** represents the **lower** bound on the number of children a node in the B Tree can have (except for the root). i.e the **minimum number** of children possible. Whereas the **Order** represents the **upper** bound on the number of children. ie. the **maximum number** possible.

            **B-tree of Order n Properties**:
            
            1. For each node x, the keys are stored in increasing order.
            1. In each node, there is a boolean value x.leaf which is true if x is a leaf.
            1. If **n** is the order of the tree, each internal node can contain at most **n - 1** keys along with a pointer to each child.
            1. Each node except root can have **at most n** children and **at least n/2** children.
            1. All leaves have the same depth (i.e. height-h of the tree).
            1. The root has at least **2** children and contains a minimum of **1** key.
            1. If **n ≥ 1**, then for any **n-key** B-tree of height h and minimum degree t ≥ 2, h ≥ logt^(n+1)/2.

        - B+ Tree

            An important concept to be understood before learning B+ tree is multilevel indexing. In multilevel indexing, the index of indices is created as in figure below.

            ![B+ Tree]({{site.baseurl}}/assets/img/2023-06-04/multilevel-indexing.webp)

            ![B+ Search Tree]({{site.baseurl}}/assets/img/2023-06-04/search-tree.webp)

            **Properties of a B+ Tree of Order m**:

            1. All leaves are at the same level.
            1. The root has **at least 2** children.
            1. Each node except root can have a maximum of **m** children and **at least m/2** children.
            1. Each node can contain a maximum of **m - 1** keys and a minimum of **⌈m/2⌉ - 1** keys.

        - Red-Black Tree

## Linear Vs Non-linear Data Structures

| Linear Data Structures    | Non Linear Data Structures |
|   :-----  |   :-----  |
| The data items are arranged in sequential order, one after the other.  | The data items are arranged in non-sequential order (hierarchical manner). |
| All the items are present on the single layer. | The data items are present at different layers. |
| It can be traversed on a single run. That is, if we start from the first element, we can traverse all the elements sequentially in a single pass. | It requires multiple runs. That is, if we start from the first element it might not be possible to traverse all the elements in a single pass. |
| The memory utilization is not efficient. | Different structures utilize memory in different efficient ways depending on the need. |
| The time complexity increase with the data size. | Time complexity remains the same.
| Example: Arrays, Stack, Queue | Example: Tree, Graph, Map |


## Comparison between a B-tree and a B+ Tree

![B+ Tree]({{site.baseurl}}/assets/img/2023-06-04/B-tree-compare.webp)

![B+ Tree]({{site.baseurl}}/assets/img/2023-06-04/B+tree-compare.webp)

The data pointers are present only at the leaf nodes on a B+ tree whereas the data pointers are present in the internal, leaf or root nodes on a B-tree.

The leaves are not connected with each other on a B-tree whereas they are connected on a B+ tree.

Operations on a B+ tree are **faster** than on a B-tree.

## Master Theorem

The master method is a formula for solving recurrence relations of the form:

```
T(n) = aT(n/b) + f(n),
where,
n = size of input
a = number of subproblems in the recursion
n/b = size of each subproblem. All subproblems are assumed to have the same size.
f(n) = cost of the work done outside the recursive call, 
      which includes the cost of dividing the problem and
      cost of merging the solutions

Here, a ≥ 1 and b > 1 are constants, and f(n) is an asymptotically positive function.
```

**Master Theorem**

If a ≥ 1 and b > 1 are constants and f(n) is an asymptotically positive function, then the time complexity of a recursive relation is given by

```
T(n) = aT(n/b) + f(n)

where, T(n) has the following asymptotic bounds:

    1. If f(n) = O(nlogb^(a-ϵ)), then T(n) = Θ(nlogb^a).

    2. If f(n) = Θ(nlogb^a), then T(n) = Θ(nlogb^a * log n).

    3. If f(n) = Ω(nlogb^(a+ϵ)), then T(n) = Θ(f(n)).

ϵ > 0 is a constant.
```

## Divide and Conquer Algorithm

- Divide: Divide the given problem into sub-problems using recursion.
- Conquer: Solve the smaller sub-problems recursively. If the subproblem is small enough, then solve it directly.
- Combine: Combine the solutions of the sub-problems that are part of the recursive process to solve the actual problem.

Applications:

- Binary Search
- Merge Sort
- Quick Sort
- Strassen's Matrix multiplication
- Karatsuba Algorithm

Advantages:

- The complexity for the multiplication of two matrices using the naive method is O(n3), whereas using the divide and conquer approach (i.e. Strassen's matrix multiplication) is O(n^2.8074). This approach also simplifies other problems, such as the Tower of Hanoi.
- This approach is **suitable for multiprocessing systems**.
- It makes efficient use of memory caches.

## Depth First Search (DFS) Algorithm

Depth first Search or Depth first traversal is a recursive algorithm for searching all the vertices of a graph or tree data structure.

The purpose of the algorithm is to mark each vertex as visited while avoiding cycles.

The DFS algorithm works as follows:

1. Start by putting any one of the graph's vertices on top of a stack.
1. Take the top item of the stack and add it to the visited list.
1. Create a list of that vertex's adjacent nodes. Add the ones which aren't in the visited list to the top of the stack.
1. Keep repeating steps 2 and 3 until the stack is empty.

Let's see how the Depth First Search algorithm works with an example. 

We use an undirected graph with 5 vertices.

![Undirected graph with 5 vertices]({{site.baseurl}}/assets/img/2023-06-04/graph-dfs-step-0.webp)

We start from vertex 0, the DFS algorithm starts by putting it in the Visited list and putting all its adjacent vertices in the **stack** (Last In First Out).

![Visit the element and put it in the visited list]({{site.baseurl}}/assets/img/2023-06-04/graph-dfs-step-1.webp)

Next, we visit the element at the top of stack i.e. 1 and go to its adjacent nodes. Since 0 has already been visited, we visit 2 instead.

![Visit the element at the top of stack]({{site.baseurl}}/assets/img/2023-06-04/graph-dfs-step-2.webp)

Vertex 2 has an **unvisited adjacent vertex** in 4, so we add that to the top of the stack and visit it.

![Visit the element at the top of stack]({{site.baseurl}}/assets/img/2023-06-04/graph-dfs-step-3.webp)

![Visit the element at the top of stack]({{site.baseurl}}/assets/img/2023-06-04/graph-dfs-step-4.webp)

After we visit the last element 3, it doesn't have any unvisited adjacent nodes, so we have completed the Depth First Traversal of the graph.

![Visit the element at the top of stack]({{site.baseurl}}/assets/img/2023-06-04/graph-dfs-step-5.webp)

## Breadth First Search (BFS) Algorithm

Breadth First Traversal or Breadth First Search is a recursive algorithm for searching all the vertices of a graph or tree data structure.

The purpose of the algorithm is to mark each vertex as visited while avoiding cycles.

The algorithm works as follows:

- Start by putting any one of the graph's vertices at the back of a queue.
- Take the front item of the queue and add it to the visited list.
- Create a list of that vertex's adjacent nodes. Add the ones which aren't in the visited list to the back of the queue.
- Keep repeating steps 2 and 3 until the queue is empty.

The graph might have two different disconnected parts so to make sure that we cover every vertex, we can also run the BFS algorithm on every node. Let's see how the Breadth First Search algorithm works with an example. 

We use an undirected graph with 5 vertices.

![Undirected graph with 5 vertices]({{site.baseurl}}/assets/img/2023-06-04/graph-bfs-step-0.webp)

We start from vertex 0, the BFS algorithm starts by putting it in the Visited list and putting all its adjacent vertices in the **queue**(First In First Out).

![Visit start vertex and add its adjacent vertices to queue]({{site.baseurl}}/assets/img/2023-06-04/graph-bfs-step-1.webp)

Next, we visit the element at the front of queue i.e. 1 and go to its adjacent nodes. Since 0 has already been visited, we visit 2 instead.

![Visit the first neighbour of start node 0, which is 1]({{site.baseurl}}/assets/img/2023-06-04/graph-bfs-step-2_2.webp)

Vertex 2 has an unvisited adjacent vertex in 4, so we add that to the back of the queue and visit 3, which is at the front of the queue.

![Visit 2 which was added to queue earlier to add its neighbours]({{site.baseurl}}/assets/img/2023-06-04/graph-bfs-step-3.webp)

![4 remains in the queue]({{site.baseurl}}/assets/img/2023-06-04/graph-bfs-step-4.webp)

Only 4 remains in the queue since the only adjacent node of 3 i.e. 0 is already visited. We visit it.

![Visit last remaining item in the queue to check if it has unvisited neighbors]({{site.baseurl}}/assets/img/2023-06-04/graph-bfs-step-5.webp)

Since the queue is empty, we have completed the Breadth First Traversal of the graph.



## Kosaraju's Algorithm

Kosaraju's Algorithm is based on the depth-first search algorithm implemented twice.

Three steps are involved.

1. Perform a depth first search on the whole graph.

    Let us start from vertex-0, visit all of its child vertices, and mark the visited vertices as done. If a vertex leads to an already visited vertex, then push this vertex to the stack.

    For example: Starting from vertex-0, go to vertex-1, vertex-2, and then to vertex-3. Vertex-3 leads to already visited vertex-0, so push the source vertex (ie. vertex-3) into the stack.

    ![DFS on the graph]({{site.baseurl}}/assets/img/2023-06-04/scc-step-1.webp)

    Go to the previous vertex (vertex-2) and visit its child vertices i.e. vertex-4, vertex-5, vertex-6 and vertex-7 sequentially. Since there is nowhere to go from vertex-7, push it into the stack.

    ![DFS on the graph]({{site.baseurl}}/assets/img/2023-06-04/scc-step-2.webp)

    Go to the previous vertex (vertex-6) and visit its child vertices. But, all of its child vertices are visited, so push it into the stack.

    ![Stacking]({{site.baseurl}}/assets/img/2023-06-04/scc-step-3.webp)

    Similarly, a final stack is created.

    ![Final Stack]({{site.baseurl}}/assets/img/2023-06-04/scc-step-43.webp)

1. Reverse the original graph.

    ![DFS on reversed graph]({{site.baseurl}}/assets/img/2023-06-04/scc-reversed-graph.webp)

1. Perform depth-first search on the reversed graph.

    Start from the top vertex of the stack. Traverse through all of its child vertices. Once the already visited vertex is reached, one strongly connected component is formed.

    For example: Pop vertex-0 from the stack. Starting from vertex-0, traverse through its child vertices (vertex-0, vertex-1, vertex-2, vertex-3 in sequence) and mark them as visited. The child of vertex-3 is already visited, so these visited vertices form one **strongly connected component** (SCC).

    ![Start from the top and traverse through all the vertices]({{site.baseurl}}/assets/img/2023-06-04/scc-reversed-step-1.webp)

    Go to the stack and pop the top vertex if already visited. Otherwise, choose the top vertex from the stack and traverse through its child vertices as presented above.

    ![Pop the top vertex if already visited]({{site.baseurl}}/assets/img/2023-06-04/reversed-step-2_0.webp)

    ![Strongly connected component]({{site.baseurl}}/assets/img/2023-06-04/reversed-step-3_0.webp)

1. Thus, the strongly connected components are:

    ![All strongly connected components]({{site.baseurl}}/assets/img/2023-06-04/scc-final-graph.webp)

## Bellman Ford's Algorithm



#### 源自[Data Structure and Algorithms](https://www.programiz.com/dsa/algorithm)