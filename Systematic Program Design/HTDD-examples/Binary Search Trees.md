---
id: Binary Search Trees
aliases:
  - Binary Search Trees
tags: []
---

# Binary Search Trees
[[Systematic Program Design#HTDD Recipe|recipe]]
> It can be used to represent arbitary data just like lists. But it is ==more efficient than lists== for searching, insertion and deletion *in large datasets*.
> It is a binary tree where each node has at most 2 children. The left child is less than the parent and the right child is greater than the parent. This property is called the binary search tree property.

## Data Definition for representing Accounts in a bank as BST nodes
```racket
(define-struct node (acc-no name left right))
;; Node is one of:
;; - false ; empty tree i.e. base case
;; - (make-node Number String Node Node) ; a node with account number, name and left and right children
;; interp. a node in a binary search tree where left child is less than the parent and right child is greater than the parent
;;         also an account number can never repeat itself
(define NB false)

(define N1 (make-node 1 "Alice" false false))
(define N2 (make-node 3 "Bob" false false))
(define N3 (make-node 2 "Charlie" N1 N2))

(define N4 (make-node 4 "Evil" N3 N7))

(define N5 (make-node 5 "Alicent" false false))
(define N6 (make-node 7 "Bobby" false false))
(define N7 (make-node 6 "Carl" N5 N6))

#;
(define (fn-for-node n)
  (cond [(false? n) (...)]
        [else
         (... (node-acc-no n)                  ; Number
              (node-name n)                    ; String
              (fn-for-node (node-left n))      ; Node
              (fn-for-node (node-right n)))])) ; Node

;; Template rules used:
;; - one of: 2 cases
;; - atomic distinct: false
;; - compound: (make-node Number String Node Node)
;; - self-reference: (fn-for-node (node-left n)) (fn-for-node (node-right n))
```

## AccountLookupInBST
Design a function that takes a binary search tree and an account number and returns the name of the account holder if the account number is present in the tree, otherwise false.

```racket
;; AccountLookupInBST : Node Number -> String or false
;; Purpose: To find the name of the account holder in the binary search tree
;;          if the account number is present in the tree, otherwise false
(check-expect (account-lookup NB 69) false) ; empty tree
(check-expect (account-lookup N4 69) false) ; invalid account number

(check-expect (account-lookup N4 4) "Evil") ; root node
(check-expect (account-lookup N4 2) "Charlie") ; left subtree
(check-expect (account-lookup N4 7) "Bobby") ; right subtree

;(define (account-lookup t n) false) ; stub

(define (account-lookup t n)
  (cond [(false? t) false]
        [else
         (cond [(= n (node-acc-no t)) (node-name t)]
               [(< n (node-acc-no t)) (account-lookup (node-left t) n)]
               [(> n (node-acc-no t)) (account-lookup (node-right t) n)])]))
```


---
