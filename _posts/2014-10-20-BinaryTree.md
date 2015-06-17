# Recover Binary Search Tree
### Recover Binary Search Tree
* Two elements of a binary search tree (BST) are swapped by mistake.

* Recover the tree without changing its structure.

* Note:
A solution using O(n) space is pretty straight forward. Could you devise a constant space solution?
#### Solution 1
```cpp
class Solution
{
public:
    void recoverTree(TreeNode *root)
    {
    	vector<TreeNode*> list;
    	vector<int > vals;
    	InOrderTravel(root, list, vals);
    	sort(vals.begin(), vals.end());
    	for(int i =0; i< list.size(); i++)
    	{
    		list[i]->val = vals[i];
    	}
    }
    void InOrderTravel(TreeNode* node, vector<TreeNode*>& list, vector<int>& vals)
    {
         if(node == NULL) return;
         InOrderTravel(node->left, list, vals);
         list.push_back(node);
         vals.push_back(node->val);
         InOrderTravel(node->right, list, vals);
    }
};
```
#### Solution 2
```cpp
class Solution
{
public:
    TreeNode *first;
    TreeNode *second;
    TreeNode *pre;

    void inOrder(TreeNode *root)
    {
        if (root==NULL)
        	return;
        else
        {
            inOrder(root->left);
            if (pre == NULL){pre = root;}
            else
            {
                if (pre->val > root->val)
                {
                    if (first==NULL)
                    	first = pre;
                    second = root;
                }
                pre = root;
            }
            inOrder(root->right);

        }
    }
    void recoverTree(TreeNode *root)
    {
        pre = NULL;
        first = NULL;
        second= NULL;
        inOrder(root);
        int val;
        val = first->val;
        first->val=second->val;
        second->val=val;
        return;
    }
};
```
####Solution 3
```cpp
class Solution {
public:
    void recoverTree(TreeNode *root)
    {
        TreeNode*pre=NULL,*first=NULL,*second=NULL,*p;
        while(root!=NULL)
        {
            if(root->left==NULL)
            {
                if(pre!= NULL && pre->val > root->val)
                {
                    if(first==NULL)
                        first=pre;
                    second=root;
                }
                pre=root;
                root=root->right;
            }
            else
            {
                p=root->left;
                while(p->right!=NULL && p->right!=root)
                    p=p->right;
                if(p->right==NULL)
                {
                    p->right=root;
                    root=root->left;
                }
                else
                {
                    p->right=NULL;
                    if(pre!= NULL && pre->val > root->val)
                    {
                        if(first==NULL)
                           first=pre;
                        second=root;
                    }
                    pre=root;
                    root=root->right;
                }
            }
        }
        if(first && second){
            first->val=first->val ^ second->val;
            second->val=first->val ^ second->val;
            first->val=first->val ^ second->val;
        }
    }
};
//http://fmarss.blogspot.com/2014/08/leetcode-solution-recover-binary-search.html
```
```cpp
class Solution {
public:
void recoverTree(TreeNode *root)
{
       TreeNode *f1=NULL, *f2=NULL;
       TreeNode  *current,*pre, *parent=NULL;

       if(root == NULL)
             return;
       bool found = false;
       current = root;
       while(current != NULL)
       {
             if(current->left == NULL)
             {
                    if(parent && parent->val > current->val)
                    {
                           if(!found)
                           {
                                 f1 = parent;
                                 found = true;
                           }
                           f2 = current;
                    }
                    parent = current;
                    current = current->right;
             }
             else
             {
                    /* Find the inorder predecessor of current */
                    pre = current->left;
                    while(pre->right != NULL && pre->right != current)
                           pre = pre->right;

                    /* Make current as right child of its inorder predecessor */
                    if(pre->right == NULL)
                    {
                           pre->right = current;
                           current = current->left;
                    }

                    /* Revert the changes made in if part to restore the original
                    tree i.e., fix the right child of predecssor */
                    else
                    {
                           pre->right = NULL;
                           if(parent->val > current->val)
                           {
                                 if(!found)
                                 {
                                        f1 = parent;
                                        found = true;
                                 }
                                 f2 = current;
                           }
                           parent = current;
                           current = current->right;
                    } /* End of if condition pre->right == NULL */
             } /* End of if condition current->left == NULL*/
       } /* End of while */

       if(f1 && f2)
             swap(f1->val, f2->val);
}
};
```
