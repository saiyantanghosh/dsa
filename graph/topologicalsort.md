* Only for direct acyclic graph
  - cycle will not work because there are no starting vertices (indegree =0).
  - in DAG at least one node has no dependency.
  - in cycle everyone waits on someone else.
* Gives the ordering of nodes based on execution orders like u->v then u appreas beafore v
* Two implementation approaches -
    - Indegree + BFS (Kahn’s Algorithm).
    - DFS + Stack.
# Key considerations
* Input graphs can have disconnected components
  - BFS Approach can handle because it is picking nodes with indegree=0 and disconnneted graphs start nodes has indegree=0. Also not visited array needed as indegree array maintaining that.
  - DFS Approach need to maintain visited array and need loop for all vertexes for disconnected components.
* No duplicate nodes
* check no of vertices - DFS call stack dependecies
  - python call stack 1000
  - java depend on stack size - 10k-50k as lightweighted than python as python add object overhead and runtime metadata.
  - It will determine whether DFS recusive can be suitable OR use DFS with stack , no call stack   
# Kahn’s Algorithm
## Algo
1. calculate indegree of all nodes.
2. choose indegree 0 first and push to queue.
3. pop node & add to answer.
4. reduce ingree of neighbours.
5. if indegree 0 push to queue.
6. repeat the steps.
```java
import java.util.*;

public class Solution {

    public int solve(int A, ArrayList<Integer> B, ArrayList<Integer> C) {

        // Adjacency list
        List<List<Integer>> adj = new ArrayList<>();

        for (int i = 0; i <= A; i++) {
            adj.add(new ArrayList<>());
        }

        // Build graph + indegree together
        int[] indegree = new int[A + 1];

        for (int i = 0; i < B.size(); i++) {

            int source = B.get(i);
            int dest = C.get(i);

            adj.get(source).add(dest);

            // Directly update indegree
            indegree[dest]++;
        }

        // Use ArrayDeque instead of LinkedList
        Queue<Integer> queue = new ArrayDeque<>();

        for (int node = 1; node <= A; node++) {

            if (indegree[node] == 0) {
                queue.offer(node);
            }
        }

        int processed = 0;

        // Kahn's Algorithm
        while (!queue.isEmpty()) {

            int current = queue.poll();
            processed++;

            for (int neighbour : adj.get(current)) {

                if (--indegree[neighbour] == 0) {
                    queue.offer(neighbour);
                }
            }
        }

        return processed == A ? 1 : 0;
    }
}
```
# DFS Topological Sort(Recursive)

## Algo
DFS+Stack
A node should be added to answer only AFTER all its neighbors are processed.

So:

DFS visit neighbors first
Then add current node to stack
Reverse stack gives topological order

1. DFS traversal 
2. Visit neighbours first
3. Push node to stack after DFS
4. Reverse stack.
# DFS Topological Sort(Iterative)
## Algo

 
# Complexity
* Both Algo T.C. - V+E
# Problems
* Course Schedule - Given prerequisites between courses, determine if all courses can be completed. - Cycle detection
  - Important Follow-ups
     - Why cycle means impossible?
     - Why topological sort only works for DAG?
     - Difference between BFS and DFS solution?
* Alien Dictionary - Given sorted words from an alien language, determine character order. - Topological Sorting
  - Important Follow-ups
       - Invalid ordering detection
       - Multiple valid answers
       - Why first differing character matters?
* Find Eventual Safe States - Find nodes that are not part of any cycle.
    - Concepts Tested
        - Reverse graph
        - Topological sort on reverse edges
        - Cycle reasoning
* Parallel Courses - Minimum semesters required to finish all courses.
    - Concepts Tested
        - Level-order BFS
        - Topological layers
        - Key Idea **Each BFS level = one semester.**
* Task Scheduler / Dependency Resolution -Tasks/services/packages depend on others.        
