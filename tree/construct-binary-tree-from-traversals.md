# Construct Binary Tree from Traversals

## [Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

Given preorder and inorder traversal of a tree, construct the binary tree.

**Note: **You may assume that duplicates do not exist in the tree.

For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
  3
 / \
9  20
  /  \
 15   7
```

### Note

找到subproblem，通过size来定位root，index就是inOrder中和root值相等的位置

在help function需要

```
int preStart, int inStart, int inEnd
```

|  | Left | Right |
| :--- | :--- | :--- |
| preStart | preStart + 1 | preStart + \(index - inStart\) + 1 |
| inStart | inStart | index - inStart + 1 |
| inEnd | index - 1 | inEnd |

这样我们就划分了不同的左右子问题

### Code

```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return helper(0, 0, inorder.length - 1, preorder, inorder);
}

private TreeNode helper(int preStart, int inStart, int inEnd,
                       int[] preorder, int[] inorder) {
    if (preStart > preorder.length - 1|| inStart > inEnd) return null;
    TreeNode root = new TreeNode(preorder[preStart]);
    int inIndex = 0;
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == root.val) {
            inIndex = i;
        }
    }
    root.left  = helper(preStart + 1, inStart, inIndex - 1, preorder, inorder);
    root.right = helper(preStart + (inIndex - inStart) + 1, inIndex + 1, inEnd, preorder, inorder);
    return root;
}
```

## [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

Given inorder and postorder traversal of a tree, construct the binary tree.

**Note: **You may assume that duplicates do not exist in the tree.

For example, given

```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

### Note

类似之前的找法，不过这次我们需要从后面往前找 \(inEnd should not larger than inStart\)

|  | Left | Right |
| :--- | :--- | :--- |
| postStart | postStart - \(inStart - Index\) - 1 | postStart - 1 |
| inStart | index - 1 | inStart |
| inEnd | inEnd | index + 1 |

### Code

```java
public TreeNode buildTree(int[] inorder, int[] postorder) {
    return buildTree(inorder, inorder.length-1, 0, postorder, postorder.length-1);
}

private TreeNode buildTree(int[] inorder, int inStart, int inEnd, int[] postorder,
		int postStart) {
	if (postStart < 0 || inStart < inEnd)
		return null;
	
	//The last element in postorder is the root.
	TreeNode root = new TreeNode(postorder[postStart]);
	
	//find the index of the root from inorder. Iterating from the end.
	int rIndex = inStart;
	for (int i = inStart; i >= inEnd; i--) {
		if (inorder[i] == postorder[postStart]) {
			rIndex = i;
			break;
		}
	}
	//build right and left subtrees. Again, scanning from the end to find the sections.
	root.right = buildTree(inorder, inStart, rIndex + 1, postorder, postStart-1);
	root.left = buildTree(inorder, rIndex - 1, inEnd, postorder, postStart - (inStart - rIndex) -1);
	return root;
}
```

## [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/description/)


