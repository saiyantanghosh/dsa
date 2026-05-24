# Problem
check if tree is BST or not
# Requirements
* are duplicate values allowed,based on it will decide conditions -  
  - left<= node <=right
* Tree Size - impact recursive stack
   - Max no of nodes 
   - screwed ?
* immutable - concurrent modification issues & thread safety  
* No of nodes = 10^7
# Approaches 
## InOrder 
* In order traversal of BST is strictly sorted 
* traverse and check whether previous element is less than next element.
* global previous pointer
### Inorder - Recursive
```java

class Solution {

    Node previousNode = null;

    public boolean isValidBST(Node root) {
        return inOrderTraversal(root);
    }

    public boolean inOrderTraversal(Node currentNode) {

        if (currentNode == null) {
            return true;
        }

        // LEFT
        if (!inOrderTraversal(currentNode.left)) {
            return false;
        }

        // ROOT - allowing strictly increasing 
        if (previousNode != null &&
            previousNode.val >= currentNode.val) {
            return false;
        }

        previousNode = currentNode;

        // RIGHT
        return inOrderTraversal(currentNode.right);
    }
}

```
### InOrder - Iterative
```java 
class Solution {

    public boolean isValidBST(Node root) {

        Stack<Node> stack = new Stack<>();

        Node current = root;
        Node previous = null;

        while (current != null || !stack.isEmpty()) {

            // Go left
            while (current != null) {
                stack.push(current);
                current = current.left;
            }

            current = stack.pop();

            // Validate ordering
            if (previous != null &&
                previous.val >= current.val) {
                return false;
            }

            previous = current;

            // Move right
            current = current.right;
        }

        return true;
    }
}
```
### Trade offs
Pros - 

* Simple Implementation

Cons -

* Duplicate not able handle because it is only checking ordering , dont able to detect whether duplicate coming from left or right tree
* T.C. - O(N) and S.C. - O(h) [Screwed - O(N) and Balanced O(logN)]
* check Preorder with range validation to solve it

## Preorder
* Range validation and Preorder(Root-Left-Right)
* start with range root (Long.MIN,Long.MAX)
  - for each subtree 
     - check lower bound (currentValue > Min )
     - check upper bound (currentVal < Max)
* key points -
  - using Long.MIN_VALUE/MAX_VALUE to avoid if Node itself can contain Integer.MIN_VALUE/MAX_VALUE
  - Not using arithmatic operation to avoid overflow, overflow when node.val + 1 ( AS Integer.MAX_VALUE+1 will Integer.MIN_VALUE and vice versa) . Doing comparison to avoid overflow
    ```java 
    validate(node.left, min, node.val - 1)

    validate(node.right, node.val + 1, max) 
    ```
### Preorder - Recursive
```java 
class Solution {

    public boolean isValidBST(Node root) {
        return validate(root,
                        Long.MIN_VALUE,
                        true,
                        Long.MAX_VALUE,
                        true);
    }

    private boolean validate(Node node,
                             long min,
                             boolean minInclusive,
                             long max,
                             boolean maxInclusive) {

        if (node == null) {
            return true;
        }

        // Check lower bound
        if (node.val < min ||
           (!minInclusive && node.val == min)) {
            return false;
        }

        // Check upper bound
        if (node.val > max ||
           (!maxInclusive && node.val == max)) {
            return false;
        }

        // LEFT: <= current
        boolean left =
            validate(node.left,
                     min,
                     minInclusive,
                     node.val,
                     true);

        // RIGHT: > current
        boolean right =
            validate(node.right,
                     node.val,
                     false,
                     max,
                     maxInclusive);

        return left && right;
    }
}

```
### Preorder - Iterative
```java 
import java.util.Stack;

class Solution {

    static class State {
        Node node;

        long min;
        boolean minInclusive;

        long max;
        boolean maxInclusive;

        State(Node node,
              long min,
              boolean minInclusive,
              long max,
              boolean maxInclusive) {

            this.node = node;

            this.min = min;
            this.minInclusive = minInclusive;

            this.max = max;
            this.maxInclusive = maxInclusive;
        }
    }

    public boolean isValidBST(Node root) {

        if (root == null) {
            return true;
        }

        Stack<State> stack = new Stack<>();

        stack.push(new State(
                root,
                Long.MIN_VALUE,
                true,
                Long.MAX_VALUE,
                true));

        while (!stack.isEmpty()) {

            State current = stack.pop();

            Node node = current.node;

            // Check lower bound
            if (node.val < current.min ||
               (!current.minInclusive &&
                 node.val == current.min)) {
                return false;
            }

            // Check upper bound
            if (node.val > current.max ||
               (!current.maxInclusive &&
                 node.val == current.max)) {
                return false;
            }

            // LEFT: <= current
            if (node.left != null) {

                stack.push(new State(
                        node.left,
                        current.min,
                        current.minInclusive,
                        node.val,
                        true));
            }

            // RIGHT: > current
            if (node.right != null) {

                stack.push(new State(
                        node.right,
                        node.val,
                        false,
                        current.max,
                        current.maxInclusive));
            }
        }

        return true;
    }
}
```
* It is memory heavy as it is creating seperate object state
* If duplicate placement can be ignored , the use Inorder Iterative Traversal 
## PostOrder
## Morris Traversal Issues
* Trees temporary mutation
* Not safe for concurrent read
* harder debugging
* exception sensitive
