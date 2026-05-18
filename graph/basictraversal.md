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
* If diff weighted then choose dijkstra(+ve weight) or Bellman Ford(-ve weight)
* Use cases 
  - Shorted path on unweighted graph
  - Nearest path 
  - Minimum Steps
```java

public void performBfs(List<List<Integer>> adjList,int V){
  boolean isVisited[] = new boolean[V+1];
  Queue<Integer> queue = new ArrayDeque<>();
  for(int i=1;i<=V;i++){
   if(!isVisited[i]){
    queue.offer(i);
    isVisited[i] = true;
   }
   while(!queue.isEmpty()){
     Integer current = queue.poll();
     for(Integer neightbour:adjList.get(current)){
      if(!isVisited[neightbour]){
        isVisited[neightbour] = true;
        queue.offer(neightbour);
      }
     }
   }
  }
}

```
T.C. O(V+E) 

S.C. - visted array & queue O(V)
# DFS Recursive
```java
public void performDfs(List<List<Integer>> adjList, int V) {

    boolean[] isVisited = new boolean[V + 1];

    // Handle disconnected components
    for (int i = 1; i <= V; i++) {

        if (!isVisited[i]) {

            dfs(adjList, isVisited, i);
        }
    }
}

private void dfs(List<List<Integer>> adjList,
                 boolean[] isVisited,
                 int current) {

    isVisited[current] = true;

    System.out.print(current + " ");

    for (int neighbour : adjList.get(current)) {

        if (!isVisited[neighbour]) {

            dfs(adjList, isVisited, neighbour);
        }
    }
}
```
# DFS Iterative

```java
public void performDfsIterative(List<List<Integer>> adjList, int V) {

    boolean[] isVisited = new boolean[V + 1];

    Stack<Integer> stack = new Stack<>();

    // Handle disconnected components
    for (int i = 1; i <= V; i++) {

        if (!isVisited[i]) {

            stack.push(i);
            isVisited[i] = true;

            while (!stack.isEmpty()) {

                int current = stack.pop();

                System.out.print(current + " ");

                for (int neighbour : adjList.get(current)) {

                    if (!isVisited[neighbour]) {

                        stack.push(neighbour);
                        isVisited[neighbour] = true;
                    }
                }
            }
        }
    }
}
```
T.C. O(V+E) 

S.C. - visted array & queue O(V)