# Adjucency List
* Save space for sparse graph(E<V^2) , for dense graph (V^2) adj matrix and adj same.
* List<List<Integer>> graph = new ArrayList<ArrayList<Integer>>();
* Build Adj. matrix
```java
List<List<Integer>> graph = buildGraph(V,edges);

private static List<List<Integer>> buildGraph(int V, int[][] edges) {
for (int i = 0; i < V; i++) {
  graph.add(new ArrayList<>());
}

// Step 2: build edges
for (int[] edge : edges) {
    int source = edge[0];
    int dest = edge[1];

     // directed graph
  graph.get(source).add(dest);
  // undirected graph both need to add u->v , v->u
  graph.get(source).add(dest);
  graph.get(dest).add(source);
}
return graph;
}

```
* Complexity
  - T.C. first for loop V and Second for loop E O(V+E)
  - S.C. each V has E entries O(V+E)
# BFS
* Use Queue to hold all neighbours
* Good for shortest/minimum related problems on unweighted graph or same weighted graph.
* If diff weighted then choose dijkstra or Bellman Ford
* Use cases 
  - Shorted path on unweighted graph
     * ss
  - Nearest path 
  - Minimum Steps
# DFS Recursive

# DFS Iterative
