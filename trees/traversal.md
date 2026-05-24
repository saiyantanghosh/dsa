# Node class
```java
class Node {
   int val;
   Node left;
   Node right;
   Node(int x) {
       val = x;
       left=null;
      right=null;
    }
}
```
# Preorder - Root Left Right
## Recursive
```java 
preOrderTraversal(root);
public void preOrderTraversal(Node current, List<Integer> output){
    if(current==null){
        return;
    }
    output.add(current.val);
    // Go left
    preOrderTraversal(current.left,output);
    // Go Right
    preOrderTraversal(current.right,output);
}
```
### Observations
* T.C. O(No of Nodes) 
* S.C. O(h) - Balanced h=log N , schrewed = N
* Call stack overflow issue - use iterative approach using Stack
## Iterative with Stack
```java
public List<Integer> preOrderTraversalIterative(Node root){
    List<Integer> output = new ArrayList<>();
    if(root==null){
        return output;
    }
   
    Deque<Node> traversal = new ArrayDeque<>();
    traversal.push(root);
    while(!traversal.isEmpty()){
        Node current = traversal.pop();
        output.add(current.val);
         // Push RIGHT first
        if (current.right != null) {
            traversal.push(current.right);
        }

        // Push LEFT second
        if (current.left != null) {
            traversal.push(current.left);
        }

    }
    return output;
}
```
### Observations
* T.C. O(No of Nodes) 
* S.C. O(h) - Balanced h=log N , schrewed = N
* Though call stackoverflow will not happened, still it is taking space. 
## Morris Traversal
* If Tree is mutable - means after to modify structure during traversal then this is taking S.C - O(1)
* check left subtree 
  - if not present , visit and move to right - as if left is not present then current node is ROOT itself.
  - if present and check any thread exists between current node with its inorder predecssor( take left subtree, go till right)
    - if thread present - VISIT node (before go left) and move to left.
    - if not present - disconnect the thread and move to right.

```java
public List<Integer> morrisPreorderTraversal(Node root){
    List<Integer> output = new ArrayList<>();
    Node current = root;
    while(current!=null){
        // check left subtree - not present
        if(current.left == null){
          output.add(current.val);
          current= current.right;
        }else{
          Node predecessor = current.left;
          //find inorder predecessor
          while(predecessor.right != null && predecessor.right != current){
            predecessor=predecessor.right;
          }
          // Create thread
          if(predecessor.right==null){
            // PREORDER VISIT
             output.add(current.val);
            predecessor.right = current;
            current = current.left;
          }else{
            predecessor.right=null;
            current = current.right;
          }
          
        }
    }
    return output;
}
```
# Inorder - Left->Root->Right
## Recursive
```java 
inOrderTraversal(root);
public void inOrderTraversal(Node current, List<Integer> output){
    if(current==null){
        return;
    }
     // Go left
    inOrderTraversal(current.left,output);

    output.add(current.val);
   
    // Go Right
    inOrderTraversal(current.right,output);
}
```
## Iterative with Stack
* Need delayed processing , after left subtree 
```java
public List<Integer> inOrderTraversalIterative(Node root){
    List<Integer> output = new ArrayList<>();
    if(root==null){
        return output;
    }
    Node current = root;
    Deque<Node> traversal = new ArrayDeque<>();
    while(current != null || !stack.isEmpty()){

        // Go completely LEFT
         while(current!=null){
            stack.push(current);
            current = current.left;
         }

        // Process node
         current = stack.pop();
         output.add(current.val);
        
        // Move RIGHT
        current = current.right;

    }
    return output;
}
```
## Morris Traversal
* delay processing , perform visit during second visit if thread exists

```java 
public List<Integer> morrisinorderTraversal(Node root){
    List<Integer> output = new ArrayList<>();
    Node current = root;
    while(current!=null){
        // check left subtree - not present
        if(current.left == null){
          output.add(current.val);
          current= current.right;
        }else{
          Node predecessor = current.left;
          //find inorder predecessor
          while(predecessor.right != null && predecessor.right != current){
            predecessor=predecessor.right;
          }
          // Create thread
          if(predecessor.right==null){
            
            predecessor.right = current;
            current = current.left;
          }else{
            // INORDER VISIT
             output.add(current.val);
            predecessor.right=null;
            current = current.right;
          }
          
        }
    }
    return output;
}
```
# Postorder - Left Right Root
## Recursive
```java 
postOrderTraversal(root);
public void postOrderTraversal(Node current, List<Integer> output){
    if(current==null){
        return;
    }
    // Go left
    postOrderTraversal(current.left,output);
    // Go Right
    postOrderTraversal(current.right,output);
    output.add(current.val);
}
```
## Iterative
### Two stack
* two stack - one stack for pre order traversal ( Root -> Right -> Left) and another stack to push in reverse order (Left,Right,Root)
```java
public List<Integer> postOrderTraversalIterative(Node root){
    List<Integer> output = new ArrayList<>();
    Deque<Node> traversal = new ArrayDeque<>();
    traversal.push(root);
    Deque<Node> reverseTraversal = new ArrayDeque<>();
    while(!traversal.isEmpty()){
         Node current = traversal.pop();
         reverseTraversal.push(current);

        // insert LEFT
         if(current.left!=null){
            traversal.push(current.left);
         }

         // insert RIGHT
         if(current.right!=null){
            traversal.push(current.right);
         }

    }

    while(!reverseTraversal.isEmpty()){
        output.add(reverseTraversal.pop().val);
    }
    return output;
}
```

T.C. - O(N)

S.C. - O(N+N) 
### One stack + Previous Pointer
* Go full left , check any right subtree exists - if exists and not previously processed then check for right subtree to finish. If right subtree already processed , then visit node and keep it as previous visited right subtree node. 
```java 

public List<Integer> postOrderTraversalIterativeOneStack(Node root){
    List<Integer> output = new ArrayList<>();
    Deque<Node> traversal = new ArrayDeque<>();
    Node current = root;
    Node previous = null;

    while(previous!=null || !traversal.isEmpty()){
        
        // Go full LEFT
         while(current.left!=null){
            traversal.push(current.left);
            current = current.left;
         }
         Node peekNode = traversal.peek();

         // check unvisited right subtree
         if (peekNode.right != null &&
            previous != peekNode.right) {

            current = peekNode.right;
        }else{
            output.add(current.val);
            previous = traversal.pop();
        }

    }

    return output;
}

```

T.C. - O(N)

S.C. - O(N)

## Morris Traversal
* Morris Preorder (visit immediately) , in order(after left subtree done) but PostOrder ( after left and right processing)
* Morris mostly top down approach predecessor.right=current but Post Order is buttomup so solution is "Reverse Right Boundary"

* Add dummy node to root for root processing
* 

```java

public List<Integer> morrisPostorderTraversal(Node root) {

    List<Integer> output = new ArrayList<>();

    Node dummy = new Node(0);
    dummy.left = root;

    Node current = dummy;

    while (current != null) {

        if (current.left == null) {

            current = current.right;
        }
        else {

            Node predecessor = current.left;

            while (predecessor.right != null &&
                   predecessor.right != current) {

                predecessor = predecessor.right;
            }

            // Create thread
            if (predecessor.right == null) {

                predecessor.right = current;

                current = current.left;
            }
            else {

                // Process reversed boundary
                collectReverse(current.left,
                               predecessor,
                               output);

                // Remove thread
                predecessor.right = null;

                current = current.right;
            }
        }
    }

    return output;
}


private void collectReverse(Node from,
                            Node to,
                            List<Integer> output) {

    reverse(from, to);

    Node current = to;

    while (true) {

        output.add(current.val);

        if (current == from) {
            break;
        }

        current = current.right;
    }

    reverse(to, from);
}

private void reverse(Node from, Node to) {

    if (from == to) {
        return;
    }

    Node previous = null;
    Node current = from;

    while (previous != to) {

        Node next = current.right;

        current.right = previous;

        previous = current;

        current = next;
    }
}


```
# Trade Offs - Morris Traversal
* only apply if trees are allowed to temporary mutable 
* Not safe for concurrent read
* harder debugging
* exception sensitive
* though while loop exists , Why Time Is Still O(n)
  - Even though predecessor search exists:

    Each edge traversed at most:

    once creating thread
    once removing thread

    So total operations remain linear.

# Tree Construction - Binary Tree From Inorder And Postorder
```java 
/**
 * Definition for binary tree
 * class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) {
 *      val = x;
 *      left=null;
 *      right=null;
 *     }
 * }
 */
public class Solution {
    public TreeNode buildTree(int[] A, int[] B) {
       return construct(B,A,0,B.length-1,0,A.length-1);

    }
    private TreeNode construct(int[] post,int[] inorder,int posts,int poste,int ins,int ine){
        
        if(posts>poste){
            return null;
        }
        int rootValue = post[poste];
        TreeNode root = new TreeNode(rootValue);
        int rootInorderIndex = 0;
        for(int i=inorder.length-1;i>=0;i--){
            if(inorder[i]==rootValue){
                rootInorderIndex =i;
                break;
            }
           
        }
        int length = rootInorderIndex - ins;
        
       root.left = construct(post,inorder,posts,posts+length-1,ins,rootInorderIndex-1);
       root.right = construct(post,inorder,posts+length,poste-1,rootInorderIndex+1,ine);
       

        return root;


    }
}
```
# Tree Construction - Binary Tree From Inorder And Preorder
```java
/**
 * Definition for binary tree
 * class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) {
 *      val = x;
 *      left=null;
 *      right=null;
 *     }
 * }
 */
public class Solution {
    public TreeNode buildTree(int[] A, int[] B) {
        int preOrderEnd=A.length;
        int inOrderEnd=B.length;
        Map<Integer,Integer> inorderIndexMapping = new HashMap<Integer,Integer>();
        for(int i=0;i<inOrderEnd;i++){
           inorderIndexMapping.put(B[i],i);
        }
        
        return constructTree(A,B,0,preOrderEnd-1,0,inOrderEnd-1,inorderIndexMapping);
    }

    private TreeNode constructTree(int[] preOrder,int[] inOrder,int preOrderStart,int preOrderEnd,int inOrderStart,int inOrderEnd,Map<Integer,Integer> inorderIndexMapping){
         
         if(preOrderStart>preOrderEnd){
             return null;
         }
         // get root
         int rootValue = preOrder[preOrderStart];
         // create root 
         TreeNode root = new TreeNode(rootValue);

         // find root index from inOrder
         int rootInorderIndex = inorderIndexMapping.get(rootValue);
         int length = rootInorderIndex-inOrderStart;
         root.left = constructTree(preOrder,inOrder,preOrderStart+1,preOrderStart+length,inOrderStart,rootInorderIndex-1,inorderIndexMapping);
         root.right = constructTree(preOrder,inOrder,preOrderStart+length+1,preOrderEnd,rootInorderIndex+1,inOrderEnd,inorderIndexMapping);
         return root;

    }
}

```
# Level Order
```java
public class Solution {
    public int[][] solve(TreeNode A) {
       
        Queue<TreeNode> traversalElements = new LinkedList<> ();
        traversalElements.add(A);
        ArrayList<int[]> result=new ArrayList<int[]>();
        
        while(!traversalElements.isEmpty()){ 
            
            int levelSize = traversalElements.size();
            int[] element=new int[levelSize];
          
            for(int i=0;i<levelSize;i++){
                TreeNode positionOne = traversalElements.poll();
                element[i] = positionOne.val;
                if(positionOne.left!=null){
                    traversalElements.add(positionOne.left);
                }
                if(positionOne.right!=null){
                    traversalElements.add(positionOne.right);
                }
            }
            result.add(element);
          
             
        }
       
        return  result.toArray(new int[result.size()][]);
    }
}

```
