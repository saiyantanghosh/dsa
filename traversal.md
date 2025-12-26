# inorder - left->root->right
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
import java.util.*;
public class Solution {
    public int[] inorderTraversal(TreeNode A) {
        List<Integer> output = new ArrayList<>();
        inorderTreeTraversal(A,output);
        int[] intArray = output.stream()
                     .mapToInt(i -> (i != null) ? i : 0)
                     .toArray();
        return  intArray;            

    }

    private void inorderTreeTraversal(TreeNode A,List<Integer> output){
        if(A==null){
            return;
        }
       
        inorderTreeTraversal(A.left,output);
         output.add(A.val);
        inorderTreeTraversal(A.right,output);
    }
}


````
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