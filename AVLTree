//   AVL Tree

package main

import "fmt"

type Node struct {
    Data int
    Height int
    Left *Node
    Right *Node
}

// Return data: O(1)
func (n *Node) GetData() int {
    return n.Data
}

// Set data: O(1)
func (n *Node) SetData(data int) {
    n.Data = data
}

// Return height: O(1)
func (n *Node) GetHeight() int {
    return n.Height
}

// Set height: O(1)
func (n *Node) SetHeight() {
    var leftheight,rightheight int
    if n.HasLeft() { leftheight = n.GetLeft().GetHeight() }
    if n.HasRight() { rightheight = n.GetRight().GetHeight() }
    if leftheight > rightheight {
        n.Height = leftheight+1
    } else {
        n.Height = rightheight+1
    }
}

func (n *Node) BalanceFactor() int {
    var leftheight,rightheight int
    if n.HasLeft() { leftheight = n.GetLeft().GetHeight() }
    if n.HasRight() { rightheight = n.GetRight().GetHeight() }
    return leftheight-rightheight
}

// Return left child: O(1)
func (n *Node) GetLeft() *Node {
    return n.Left
}

// Set left child: O(1)
func (n *Node) SetLeft(left *Node) {
    n.Left = left
}

// Test whether node has left child: O(1)
func (n *Node) HasLeft() bool {
    if n.Left == nil { return false }
    return true
}

// Return right child: O(1)
func (n *Node) GetRight() *Node {
    return n.Right
}

// Set right child: O(1)
func (n *Node) SetRight(right *Node) {
    n.Right = right
}

// Test whether node has right child: O(1)
func (n *Node) HasRight() bool {
    if n.Right == nil { return false }
    return true
}

type AVLTree struct {
    Root *Node
}

// Create a new AVL Tree: O(1)
func NewAVLTree() *AVLTree {
    return &AVLTree{}
}

func (a *AVLTree) Search(data int) bool {
    if a.Root == nil { return false }
    current := a.Root
    for {
        if data < current.GetData() {
            if current.HasLeft() {
                current = current.GetLeft()
            } else {
                break
            }
        } else if data > current.GetData(){
            if current.HasRight() {
                current = current.GetRight()
            } else {
                break
            }
        } else {
            return true
        }
    }
    return false
}

func (a *AVLTree) SearchMin(current *Node) int {
    if current.HasLeft() { return a.SearchMin(current.GetLeft()) }
    return current.GetData()
}

func (a *AVLTree) RotateRight(current *Node) *Node {
    left := current.GetLeft()
    current.SetLeft(left.GetRight())
    left.SetRight(current)
    current.SetHeight()
    return left
}

func (a *AVLTree) RotateLeft(current *Node) *Node {
    right := current.GetRight()
    current.SetRight(right.GetLeft())
    right.SetLeft(current)
    current.SetHeight()
    return right
}

func (a *AVLTree) Rebalance(current *Node) *Node {
    b := current.BalanceFactor()
    if b > 1 {
        if current.GetLeft().BalanceFactor() < 0 {
            current.SetLeft(a.RotateLeft(current.GetLeft()))
        }
        current = a.RotateRight(current)
    } else if b < -1 {
        if current.GetRight().BalanceFactor() > 0 {
            current.SetRight(a.RotateRight(current.GetRight()))
        }
        current = a.RotateLeft(current)
    }
    current.SetHeight()
    return current
}

func (a *AVLTree) Insert(data int) {
    newnode := &Node{data,1,nil,nil}
    if a.Root == nil { a.Root = newnode; return; }
    a.Root = a.insert(a.Root,newnode)
}

func (a *AVLTree) insert(current, newnode *Node) *Node {
    if newnode.GetData() < current.GetData() {
        if current.HasLeft() {
            current.SetLeft(a.insert(current.GetLeft(),newnode))
        } else {
            current.SetLeft(newnode)
        }
    } else if newnode.GetData() > current.GetData() {
        if current.HasRight() {
            current.SetRight(a.insert(current.GetRight(),newnode))
        } else {
            current.SetRight(newnode)
        }
    } else {
        return nil
    }
    current.SetHeight()
    current = a.Rebalance(current)
    return current
}

func (a *AVLTree) Delete(data int) {
    if a.Root == nil { return }
    if a.Root.GetData() == data {
        auxroot := &Node{0,0,a.Root,nil}
        a.delete(a.Root,auxroot,data)
        a.Root = auxroot.GetLeft()
    } else {
        a.delete(a.Root,nil,data)
    }
}

func (a *AVLTree) delete(current, parent *Node, data int) *Node {
    if data < current.GetData() {
        if current.GetLeft() == nil { return nil }
        current.SetLeft(a.delete(current.GetLeft(),current,data))
    } else if data > current.GetData() {
        if current.GetRight() == nil { return nil }
        current.SetRight(a.delete(current.GetRight(),current,data))
    } else {
        if current.HasLeft() && current.HasRight() {
            current.SetData(a.SearchMin(current.GetRight()))
            current.SetRight(a.delete(current.GetRight(),current,current.GetData()))
        } else if parent.GetLeft() == current {
            if current.HasLeft() {
                parent.SetLeft(current.GetLeft())
                return current.GetLeft()
            } else {
                parent.SetLeft(current.GetRight())
                return current.GetRight()
            }
        } else if parent.GetRight() == current {
            if current.HasRight() {
                parent.SetRight(current.GetRight())
                return current.GetRight()
            } else {
                parent.SetRight(current.GetLeft())
                return current.GetLeft()
            }
        }
    }
    current.SetHeight()
    current = a.Rebalance(current)
    return current
}

// Print BST with BFS: O(n)
func (a *AVLTree) BFSPrinting() {
    if a.Root == nil { return }
    q := make([]*Node,0,0)
    q = append(q,a.Root)
    for len(q) != 0 {
        front := q[0]
        q = q[1:]
        fmt.Print(front.GetData()," ")
        if front.HasLeft() { q = append(q,front.GetLeft()) }
        if front.HasRight() { q = append(q,front.GetRight()) }
    }
    fmt.Println()
}

func main() {
    a := NewAVLTree()
    for i := 1; i <= 10; i++ {
        a.Insert(i)
    }
    a.BFSPrinting()
    a.Delete(9)
    a.BFSPrinting()
    a.Delete(10)
    a.BFSPrinting()
}
