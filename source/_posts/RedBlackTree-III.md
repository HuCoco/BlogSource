---
title: RedBlackTree_III
date: 2018-03-02 23:38:19
thumbnail: http://hucoco.com/img/RedBlackTree/rbtree.png
tags: 
- C++
- Data Structure
- Tree
categories:
- Data Structure
- Tree
---

[RedBlackTree III Chinese version address](http://hucoco.coding.me/2016/11/10/Tree/RBTree/%E7%BA%A2%E9%BB%91%E6%A0%91%E5%AE%9E%E7%8E%B0%EF%BC%88%E5%88%A0%E9%99%A4%EF%BC%89/)

[The Last Section](http://hucoco.com/2018/03/02/RedBlackTree-II/)

## Basic operation of red black trees 

### Delete

The method of deleting a node is the same as that of deleting a node in a binary search tree.

#### Binary search tree Delete pseudo code

```C++
TREE-DELETE(T, z)  
  if left[z] = NIL or right[z] = NIL  
      then y ← z  
      else y ← TREE-SUCCESSOR(z)  
  if left[y] ≠ NIL  
      then x ← left[y]  
      else x ← right[y]  
  if x ≠ NIL  
      then p[x] ← p[y]  
  if p[y] = NIL  
      then root[T] ← x  
      else if y = left[p[y]]  
              then left[p[y]] ← x  
              else right[p[y]] ← x  
  if y ≠ z  
      then key[z] ← key[y]  
           copy y's satellite data into z  
  return y
```

<!--more-->

According to the node to be deleted according to the number of sons can be divided into three cases

1. No child nodes. just set the child node of the parent node to NULL and delete the child node.
2. Only one child node, the parent node's child node pointer points to the grandson node, delete the son node.
3. Two child nodes, After deleting the node, but also to ensure that the search binary tree structure. In this case, the largest element in the left child node or the smallest element in the right child node can be placed in the position of the node to be deleted to ensure the structure is unchanged. 

#### Binary search tree Delete pseudo code

```C++
RB-DELETE(T, z)
if left[z] = nil[T] or right[z] = nil[T]         
   then y ← z                                  
   else y ← TREE-SUCCESSOR(z)                  
if left[y] ≠ nil[T]
   then x ← left[y]                            
   else x ← right[y]                           
p[x] ← p[y]                                    
if p[y] = nil[T]                               
   then root[T] ← x                            
   else if y = left[p[y]]                    
           then left[p[y]] ← x                 
           else right[p[y]] ← x                
if y ≠ z                                    
   then key[z] ← key[y]                        
        copy y's satellite data into z         
if color[y] = BLACK                            
   then RB-DELETE-FIXUP(T, x)                  
return y
```

After you delete a node, it may violate the feature of the red black tree.

1. If you delete the red node, then the feature of red black tree remains. At this time do not do the correction operation.
2. If the deleted node is a black node, the feature of the red black tree may be changed, and we want to make corrections to it

So what are the feature of trees that will change?

1. If the delete node is not the only node in the tree, then the number of black nodes to each leaf node of the delete node will change, and the property 5 is destroyed.
2. If the deleted node's only non-empty child node is red, and the deleted node's parent node is also red, then the feature of 4 is destroyed.
3. If the deleted node is the root node and its only non-empty child node is red, the new root node will be red after deletion, violating property 2.

The above repair looks a bit complicated, here we use an analytical skills:

begin to adjust the node from which the node was removed later, And think it has an extra black color. What is the meaning of an extra black here? We do not add the nodes of the red and black trees to another color of red and black. Here is just an assumption, and we think we are currently pointing to it, so there is an extra black. It can be thought that its black color is inherited from its parent after it was deleted. It can now hold two colors. If it was originally red, then it is red + black, and if it is black then its current color It is black + black. With this extra black, the feature of red-black tree 5 will remain unchanged. Now it is enough to resume other things as far as possible, and try to move all the possibilities to the root as far as possible.

If it is the case, the recovery is relatively simple:

###### The current node is red + black.

The current node is set to black, the end of the red-black tree at this time the nature of all recovery.

###### The current node is black + black and is the root node.

do nothing, the end.

But the following situation, it will be more complicated

|Case|Description|
|:-:|:--|
|1|The current node is black + black and the sibling node is red (in this case, the children of the parent node and sibling nodes are divided into black).|
|2|The current node is black and black and the brother is black and both siblings have black children.|
|3|The current node color is black + black, brother node is black, brother's left child is red, the right child is black.|
|4|The current node color is black-black, its sibling node is black, but the sibling node's right child is red, the sibling node's left child's color is arbitrary|

#### Case Description

#### Case 1

##### Description

The current node is black + black and the sibling node is red (in this case, the children of the parent node and sibling nodes are divided into black).

##### Method

1. Set the parent node to red.
2. Set the brother nodes to black.
3. Do left rotation on parent node.
4. reset the brother nodes.

###### pseudo code

```
while x ≠ root[T] and color[x] = BLACK    
    do if x = left[p[x]]    
          then w ← right[p[x]]    
               if color[w] = RED    
                  then color[w] ← BLACK                 ▹  Case 1    
                       color[p[x]] ← RED                ▹  Case 1    
                       LEFT-ROTATE(T, p[x])             ▹  Case 1    
                       w ← right[p[x]]                  ▹  Case 1

```

![Before Handle Case 1](http://hucoco.com/img/RedBlackTree/removeFix1.jpg)

![After Handle Case 1](http://hucoco.com/img/RedBlackTree/removeFix2.jpg)

#### Case 2

##### Description

The current node is black and black and the brother is black and both siblings have black children.

##### Method

1. Set brother node to red.
2. Set current node to parent node.

###### pseudo code

```
if color[left[w]] = BLACK and color[right[w]] = BLACK    
   then color[w] ← RED                          ▹  Case 2    
        x ← p[x]                                ▹  Case 2
```

![Before Handle Case 2](http://hucoco.com/img/RedBlackTree/removeFix3.jpg)

![After Handle Case 2](http://hucoco.com/img/RedBlackTree/removeFix4.jpg)

#### Case 3

##### Description

The current node color is black + black, brother node is black, brother's left child is red, the right child is black.

##### Method

1. Set brother node's left child node to black.
2. Set brother node to red.
3. Do right rotation on brother node.
4. reset brother node.

###### pseudo code

```
else if color[right[w]] = BLACK    
        then color[left[w]] ← BLACK          ▹  Case 3    
             color[w] ← RED                  ▹  Case 3    
             RIGHT-ROTATE(T, w)              ▹  Case 3    
             w ← right[p[x]]                 ▹  Case 3
```

![Before Handle Case 2](http://hucoco.com/img/RedBlackTree/removeFix5.jpg)

![After Handle Case 2](http://hucoco.com/img/RedBlackTree/removeFix6.jpg)

#### Case 4

##### Description

The current node color is black-black, its sibling node is black, but the sibling node's right child is red, the sibling node's left child's color is arbitrary

##### Method

1. Set the brother node to the color of parent node.
2. Set parent node to black.
3. Set the brother node's right left node to black.
4. Do right rotation on parent node.
5. set current node to root node.

###### pseudo code

```
color[w] ← color[p[x]]                 ▹  Case 4    
color[p[x]] ← BLACK                    ▹  Case 4    
color[right[w]] ← BLACK                ▹  Case 4    
LEFT-ROTATE(T, p[x])                   ▹  Case 4    
x ← root[T]                            ▹  Case 4 
```

![Before Handle Case 2](http://hucoco.com/img/RedBlackTree/removeFix7.jpg)

![After Handle Case 2](http://hucoco.com/img/RedBlackTree/removeFix8.jpg)
