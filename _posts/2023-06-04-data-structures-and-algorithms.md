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

        **Graph Representation**

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

- Start by putting any one of the graph's vertices at the back of a *queue*.
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

    ![Final Stack]({{site.baseurl}}/assets/img/2023-06-04/scc-step-4.webp)

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

Bellman Ford algorithm works by overestimating the length of the path from the starting vertex to all other vertices. Then it iteratively relaxes those estimates by finding new paths that are shorter than the previously overestimated paths.

By doing this repeatedly for all vertices, we can guarantee that the result is optimized.

![Step-1 for Bellman Ford's algorithm]({{site.baseurl}}/assets/img/2023-06-04/Bellman-Ford-Algorithm-1.webp)

![Step-2 for Bellman Ford's algorithm]({{site.baseurl}}/assets/img/2023-06-04/Bellman-Ford-Algorithm-2.webp)

![Step-3 for Bellman Ford's algorithm]({{site.baseurl}}/assets/img/2023-06-04/Bellman-Ford-Algorithm-3.webp)

![Step-4 for Bellman Ford's algorithm]({{site.baseurl}}/assets/img/2023-06-04/Bellman-Ford-Algorithm-4.webp)

![Step-5 for Bellman Ford's algorithm]({{site.baseurl}}/assets/img/2023-06-04/Bellman-Ford-Algorithm-5.webp)

![Step-6 for Bellman Ford's algorithm]({{site.baseurl}}/assets/img/2023-06-04/Bellman-Ford-Algorithm-6.webp)

## Bellman Ford vs Dijkstra

Bellman Ford's algorithm and Dijkstra's algorithm are very similar in structure. While Dijkstra looks only to the immediate neighbors of a vertex, Bellman goes through each edge in every iteration.

![Bellman Ford's Algorithm vs Dijkstra's Algorithm]({{site.baseurl}}/assets/img/2023-06-04/bellman-ford-vs-dijkstra.webp)

## Sorting and Searching Algorithms

- Bubble Sort

    Bubble sort is a sorting algorithm that compares two adjacent elements and swaps them until they are in the intended order.

    - Time Complexity: Best Case = Ω(N), Worst Case = O(N^2), Average Case = Θ(N^2)
    - Space Complexity: Worst Case = O(1)

- Selection Sort

    Selection sort is a sorting algorithm that selects **the smallest element** from an unsorted list in each iteration and places that element **at the beginning** of the unsorted list.

    ```python
    # Selection sort in Python
    def selectionSort(array, size):
    
        for step in range(size):
            min_idx = step

            for i in range(step + 1, size):
            
                # to sort in descending order, change > to < in this line
                # select the minimum element in each loop
                if array[i] < array[min_idx]:
                    min_idx = i
            
            # put min at the correct position
            (array[step], array[min_idx]) = (array[min_idx], array[step])

    data = [-2, 45, 0, 11, -9]
    size = len(data)
    selectionSort(data, size)
    print('Sorted Array in Ascending Order:')
    print(data)
    ```

    - Time Complexity: Best Case = Ω(N^2), Worst Case = O(N^2), Average Case = Θ(N^2)
    - Space Complexity: Worst Case = O(1)

- Insertion Sort

    Insertion sort is a sorting algorithm that places an unsorted element **at its suitable place** in each iteration.

    ```python
    # Insertion sort in Python
    def insertionSort(array):

        for step in range(1, len(array)):
            key = array[step]
            j = step - 1
            
            # Compare key with each element on the left of it until an element smaller than it is found
            # For descending order, change key<array[j] to key>array[j].        
            while j >= 0 and key < array[j]:
                array[j + 1] = array[j]
                j = j - 1
            
            # Place key at after the element just smaller than it.
            array[j + 1] = key

    data = [9, 5, 1, 4, 3]
    insertionSort(data)
    print('Sorted Array in Ascending Order:')
    print(data)
    ```

    - Time Complexity: Best Case = Ω(N), Worst Case = O(N^2), Average Case = Θ(N^2)
    - Space Complexity: Worst Case = O(1)

- Merge Sort

    Merge Sort is one of the most popular sorting algorithms that is based on the principle of Divide and Conquer Algorithm.

    Here, a problem is divided into multiple sub-problems. Each sub-problem is solved individually. Finally, sub-problems are combined to form the final solution.

    ![Merge Sort example]({{site.baseurl}}/assets/img/2023-06-04/merge-sort-example_0.png)

    ```python
    # MergeSort in Python
    def mergeSort(array):
        if len(array) > 1:

            #  r is the point where the array is divided into two subarrays
            r = len(array)//2
            L = array[:r]
            M = array[r:]

            # Sort the two halves
            mergeSort(L)
            mergeSort(M)

            i = j = k = 0

            # Until we reach either end of either L or M, pick larger among
            # elements L and M and place them in the correct position at A[p..r]
            while i < len(L) and j < len(M):
                if L[i] < M[j]:
                    array[k] = L[i]
                    i += 1
                else:
                    array[k] = M[j]
                    j += 1
                k += 1

            # When we run out of elements in either L or M,
            # pick up the remaining elements and put in A[p..r]
            while i < len(L):
                array[k] = L[i]
                i += 1
                k += 1

            while j < len(M):
                array[k] = M[j]
                j += 1
                k += 1

    # Print the array
    def printList(array):
        for i in range(len(array)):
            print(array[i], end=" ")
        print()


    # Driver program
    if __name__ == '__main__':
        array = [6, 5, 12, 10, 9, 1]

        mergeSort(array)

        print("Sorted array is: ")
        printList(array)
    ```

    - Time Complexity: Best Case = Ω(n log(n)), Worst Case = O(n log(n)), Average Case = Θ(n log(n))
    - Space Complexity: Worst Case = O(n)

- Quicksort

    Quicksort is a sorting algorithm based on the divide and conquer approach where

    1. An array is divided into subarrays by selecting a pivot element (element selected from the array). While dividing the array, the pivot element should be positioned in such a way that elements less than pivot are kept on the left side and elements greater than pivot are on the right side of the pivot.
    1. The left and right subarrays are also divided using the same approach. This process continues until each subarray contains a single element.
    1. At this point, elements are already sorted. Finally, elements are combined to form a sorted array.

    - Time Complexity: Best Case = Ω(n log(n)), Worst Case = O(n^2), Average Case = Θ(n log(n))
    - Space Complexity: Worst Case = O(n)

- Counting Sort

    Counting sort is a sorting algorithm that sorts the elements of an array by counting the number of occurrences of each unique element in the array. The count is stored in an auxiliary array and the sorting is done by mapping the count as an index of the auxiliary array.

    ![Counting sort]({{site.baseurl}}/assets/img/2023-06-04/Counting-sort-4_1.png)

    ```python
    def counting_sort(array):
        """
        Counting sort is a sorting algorithm that sorts the elements of an array by counting the number of occurrences of each element.

        Args:
            array: The array to be sorted.

        Returns:
            A sorted array.
        """

        # Find the maximum element in the array.
        max_element = max(array)
        size = len(array)

        # Create a count array to store the number of occurrences of each element.
        count_array = [0] * (max_element + 1)

        # Increment the count of each element in the count array.
        for element in array:
            count_array[element] += 1

        # For each element in the countArray, sum up its value with the value of the previous 
        # element, and then store that value as the value of the current element
        for i in range(1, max_element + 1):
            count_array[i] += count_array[i - 1]

        # Create a sorted array by adding the elements from the count array.
        sorted_array = [0] * size
        i = size - 1
        while i >= 0:
            sorted_array[count_array[current_element] - 1] = array[i]
            count_array[array[i]] -= 1
            i -= 1

        return sorted_array
        ```

    - Time Complexity: Best Case = Ω(n + k), Worst Case = O(n + k), Average Case = Θ(n + k)
    - Space Complexity: Worst Case = O(k)

- Radix Sort+

    Radix sort is a sorting algorithm that sorts the elements by first grouping the individual digits of the same place value. Then, sort the elements according to their increasing/decreasing order.

    - Time Complexity: Best Case = Ω(nk), Worst Case = O(nk), Average Case = Θ(nk)
    - Space Complexity: Worst Case = O(n + k)

- Bucket Sort
- Heap Sort
- Shell Sort
- Linear Search
- Binary Search

## Time Complexities of all Sorting Algorithms

|Algorithm	|Time Complexity	              |||Space Complexity|
|-- 	    |Best	|Average	|Worst	        |Worst|
|:--        |:--    |:--        |:--            |:--  |
|Selection Sort	|Ω(n^2)	|θ(n^2)	|O(n^2)	|O(1)|
|Bubble Sort	|Ω(n)	|θ(n^2)	|O(n^2)	|O(1)|
|Insertion Sort	|Ω(n)	|θ(n^2)	|O(n^2)	|O(1)|
|Heap Sort	|Ω(n log(n))	|θ(n log(n))	|O(n log(n))	|O(1)|
|Quick Sort	|Ω(n log(n))	|θ(n log(n))	|O(n^2)	        |O(n)|
|Merge Sort	|Ω(n log(n))	|θ(n log(n))	|O(n log(n))	|O(n)|
|Bucket Sort	|Ω(n + k)	|θ(n + k)	    |O(n^2)	        |O(n)|
|Radix Sort	|Ω(nk)	        |θ(nk)	        |O(nk)	        |O(n + k)|
|Count Sort	|Ω(n + k)	|θ(n + k)	|O(n + k)	|O(k)|
|Shell Sort	|Ω(n log(n))	|θ(n log(n))	|O(n^2)	|O(1)|
|Tim Sort	|Ω(n)	|θ(n log(n))	|O(n log (n))	|O(n)|
|Tree Sort	|Ω(n log(n))	|θ(n log(n))	|O(n^2)	|O(n)|
|Cube Sort	|Ω(n)	|θ(n log(n))	|O(n log(n))	|O(n)|

## Greedy Algorithm

A greedy algorithm is an approach for solving a problem by selecting the best option available at the moment. It doesn't worry whether the current best result will bring the overall optimal result.

The algorithm never reverses the earlier decision even if the choice is wrong. It works in a top-down approach.

1. To begin with, the solution set (containing answers) is empty.
1. At each step, an item is added to the solution set until a solution is reached.
1. If the solution set is feasible, the current item is kept.
1. Else, the item is rejected and never considered again.

## Ford-Fulkerson Algorithm

Ford-Fulkerson algorithm is a greedy approach for calculating the maximum possible flow in a network or a graph.

A term, **flow network**, is used to describe a network of vertices and edges with a **source (S)** and a **sink (T)**. Each vertex, except **S** and **T**, can receive and send an equal amount of stuff through it. **S** can only send and **T** can only receive stuff.

We can visualize the understanding of the algorithm using a flow of liquid inside a network of pipes of different capacities. Each pipe has a certain capacity of liquid it can transfer at an instance. For this algorithm, we are going to find how much liquid can be flowed from the source to the sink at an instance using the network.

![Flow network graph]({{site.baseurl}}/assets/img/2023-06-04/flow-network.webp)

**Augmenting Path**: It is the path available in a flow network.

**Residual Graph**: It represents the flow network that has additional possible flow.

**Residual Capacity**: It is the capacity of the edge after subtracting the flow from the maximum capacity.

1. Initialize the flow in all the edges to 0.
1. While there is an augmenting path between the source and the sink, add this path to the flow.
1. Update the residual graph.

![Flow network graph example]({{site.baseurl}}/assets/img/2023-06-04/flow-network-example.webp)

Select any arbitrary path from S to T. In this step, we have selected path S-A-B-T.

![Find a path]({{site.baseurl}}/assets/img/2023-06-04/flow-network-1.webp)

The minimum capacity among the three edges is 2 (B-T). Based on this, update the flow/capacity for each path.

![Update the capacities]({{site.baseurl}}/assets/img/2023-06-04/flow-network-1-update.webp)

Select another path S-D-C-T. The minimum capacity among these edges is 3 (S-D).

![Find next path]({{site.baseurl}}/assets/img/2023-06-04/flow-network-2.webp)

Update the capacities according to this.

![Update the capacities]({{site.baseurl}}/assets/img/2023-06-04/flow-network-2-update.webp)

Now, let us consider the reverse-path B-D as well. Selecting path S-A-B-D-C-T. The minimum residual capacity among the edges is 1 (D-C).

![Find next path]({{site.baseurl}}/assets/img/2023-06-04/flow-network-3.webp)

Updating the capacities.

![Update the capacities]({{site.baseurl}}/assets/img/2023-06-04/flow-network-3-update.webp)

The capacity for forward and reverse paths are considered separately.

Adding all the flows = 2 + 3 + 1 = 6, which is the maximum possible flow on the flow network.

> Note that if the capacity for any edge is full, then that path cannot be used.



#### 源自[Data Structures and Algorithms](https://www.programiz.com/dsa/algorithm)