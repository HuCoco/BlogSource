---
title: RedBlackTree I
date: 2018-03-02 23:38:12
thumbnail: http://hucoco.com/img/RedBlackTree/rbtree.png
tags: 
- C++
- Data Structure
- Tree
categories:
- Data Structure
- Tree
---

[RedBlackTree I Chinese version address](http://hucoco.coding.me/2016/11/08/Tree/RBTree/%E7%BA%A2%E9%BB%91%E6%A0%91/)

## Binary Tree Review

There are several features of binary tree:

1.  if the left subtree of any nodes is not null, the value of all nodes on the left subtree is less than the value of its root node.
2.  if the right subtree of any nodes is not null, the value of all nodes on the right subtree is greater than the value of its root node.
3.  the left and right subtrees of any node are also binary tree.
4.  No nodes with equal values

## Red Black Tree Overview

Red Black Tree is a special binary tree. each node of a red black tree has a bit to represenet the color, which can be red or black.

<!--more-->

### Red Black Tree Features

1. the color of each node is red or black.
2. the color of root node is black.
3. the color of each leaf node is black.
4. if a node is red, the subnode of it must be black.
5. The same number of black nodes is included on all paths from a node to the node's descendant node.

![RedBlackTree](http://hucoco.com/img/RedBlackTree/rbtree.png)

#### Notes:

1. leaf node is means which node is null.
2. Feature 5 to ensure that no path is twice as long as the other path. Thus, the red black tree is a relatively balanced binary tree.

## Application of red black trees

The application of the red-black tree is particularly extensive, mainly because it uses it to store ordered data. Its time complexity is O (lgn), which is very efficient.

But its implementation is complicated, and insert, delete operation will pay more cost.

* Set and Map in STL
* TreeSet and TreeMap in Java
* Virtual memory management in Linux

## Basic operation of red black trees 

### Left Rotation

```
                              z
  x                          /                  
 / \      --(左旋)-->        x
y   z                      /
                          y
```

Make left rotation on x node, and it will be a left node.

![Left Rotate](http://hucoco.com/img/RedBlackTree/leftrotate.jpg)

#### Pseudo code

```
LEFT-ROTATE(T, x)  
 y ← right[x]            
 right[x] ← left[y]      
 p[left[y]] ← x         
 p[y] ← p[x]             
 if p[x] = nil[T]       
 then root[T] ← y                 
 else if x = left[p[x]]  
           then left[p[x]] ← y   
           else right[p[x]] ← y  
 left[y] ← x             
 p[x] ← y
```

#### C++ Code

```
template <class T>
void RedBlackTree<T>::leftRotate(RedBlackNode<T> *&root, RedBlackNode<T> *x) {

	RedBlackNode<T> *y = x->pRight;     

	x->pRight = y->pLeft;                    
	if(y->pLeft != nullptr) {
		y->pLeft->pParent = x;        
	}

	y->pParent = x->pParent;         

	if(x->pParent == nullptr) {      
		root = y;                      
	}
	else {
		if(x->pParent->pLeft == x) {    
			x->pParent->pLeft = y;      
		}
		else {
			x->pParent->pRight = y; 
		}
	}
	y->pLeft = x;                   
	x->pParent = y;                    
}                                                                                                            
```

### Right Rotation

```
                             y
  x                           \                 
 / \      --(右旋)-->           x
y   z                            \
                                  z
```

Make right rotation on x node, and it will be a right node.

![](http://hucoco.com/img/RedBlackTree/rightrotate.jpg)

#### Pseudo code

```
RIGHT-ROTATE(T, y)  
 x ← left[y]             
 left[y] ← right[x]      
 p[right[x]] ← y         
 p[x] ← p[y]             
 if p[y] = nil[T]       
 then root[T] ← x               
 else if y = right[p[y]]  
           then right[p[y]] ← x   
           else left[p[y]] ← x   
 right[x] ← y            
 p[y] ← x
```

#### C++ Code

```
template <class T>
void RedBlackTree<T>::rightRotate(RedBlackNode<T> *&root, RedBlackNode<T> *y) {

	RedBlackNode<T> *x = y->pLeft;    

	y->pLeft = x->pRight;              
	if(x->pRight != nullptr) {
		x->pRight->pParent = y;     
	}

	x->pParent = y->pParent;           

	if(y->pParent == nullptr) {        
		root = x;                       
	}
	else {
		if(y->pParent->pRight == y) {   
			y->pParent->pRight = x;     
		}
		else {
			y->pParent->pLeft = x;   
		}
	}

	x->pRight = y;       
	y->pParent = x;  
}
```

[The Next Section](http://hucoco.com/2018/03/02/RedBlackTree-II/)