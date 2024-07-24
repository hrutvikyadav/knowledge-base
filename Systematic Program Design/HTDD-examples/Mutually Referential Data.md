---
id: Mutually Referential Data
aliases:
  - Mutually Referential Data
tags: []
---

# Mutually Referential Data
==Recipe is updated for mutually referential data.==
#htdd #mutually-referential-data #arbitrary-arity-tree

> [!tip] There is certain correspondence between external; self; and mutual references.
> We should be able to predict and identify them in the data definition.

> [!info] Mutually self-referential types can represent a tree of arbitrary depth and width.
> It is called `arbitrary arity` because it can be *arbitrarily wide* at **each level**, as well as *arbitrarily deep*.
> [[Binary Search Trees]] on the other hand can *==only== have 2 children at each level.*

> [!warning] The **key** here is to *identify the `mutual reference` cycles and `self reference` cycles* in the data definition.
> This is **important for the templating** part of the recipe.

> [!info] A general rule of thumb when designing data involving mututally referential types is to *do the Type comment, interp, examples **==for the types involved==** together*

## Example Problem Statement

### FileSystem
#data-definition
Design a data definition to represent a file system. A file system is a tree of directories and files. Each directory can contain files and other directories.
```racket
(define-struct file (name data subs))
;; File is (make-file String Integer (ListofFiles))
;; interp. a file with name, data and a list of files in it
;;        File content i.e. data can be a single integer and it is a leaf node in the tree
;;        If a file is a directory, it can have children ( other files and directories ) and the data is 0

;; ListofFiles is one of:
;; - empty
;; - (cons File ListofFiles)
;; interp. a list of Files

(define F1 (make-file "file1" 1 empty))       ; LEAF
(define F2 (make-file "file2" 2 empty))       ; LEAF
(define F3 (make-file "file3" 3 empty))       ; LEAF
(define F4 (make-file "dir4" 0 (list F1 F2))) ; LEVEL 1
(define F5 (make-file "dir5" 0 (list F3)))    ; LEVEL 1
(define F6 (make-file "dir6" 0 (list F4 F5))) ; ROOT

#;
(define ( fn-for-file f )
    (... (file-name f)    ; String
         (file-data f)    ; Integer
         (fn-for-listoffiles (file-subs f)))) ; ListofFiles

(define ( fn-for-listoffiles lof )
    (cond [(empty? lof) (...)]
          [else
           (... (fn-for-file (first lof))
                (fn-for-listoffiles (rest lof)))]))
```

### SumOfFiles
> [!info] A general rule of thumb when designing a function involving mututally referential types is to *design the ==functions== together*

Design a function that takes a file system and returns the sum of all the data in the files.
```racket
;; File -> Integer
;; ListofFiles -> Integer
;; Purpose: To find the sum of all the data in the files in the file system
(check-expect (sum-of-files F6) 6) ; 0 + 1 + 2 + 3
(check-expect (sum-of-files F4) 3) ; 0 + 1 + 2
(check-expect (sum-of-files F5) 3) ; 0 + 3
(check-expect (sum-of-files F1) 1) ; 0 + 1

;(define (sum-of-file f) 0) ; stub
;(define (sum-of-listoffiles lof) 0) ; stub

(define ( sum-of-file f )
    (if (zero? (file-data f))
        (sum-of-listoffiles (file-subs f))
        (file-data f)))

(define ( sum-of-listoffiles lof )
    (cond [(empty? lof) 0]
          [else
           (+ (sum-of-file (first lof))
              (sum-of-listoffiles (rest lof)))]))
```

### SearchForFile
#backtracking #tree-traversal
Design a function that takes a file and a file name and returns the file data with the given name or false if the file is not found.
```racket
;; File String -> Integer or false
;; ListofFiles String -> Integer or false
;; Purpose: To find the file with the given name in the file system
(check-expect (search-for-list-of-files "file1" empty) false) ; "file1" is not present in empty
(check-expect (search-for-list-of-files "file1" (list F1 F2)) 1) ; "file1" is present in (list F1 F2
(check-expect (search-for-file F2 "file1") false) ; "file1" is not present in F2
(check-expect (search-for-file F1 "file1") 1) ; "file1" is present in F1
(check-expect (search-for-file F6 "file1") 1) ; "file1" is present in F6

; (define (search-for-file f n) false) ; stub
; (define (search-for-files f n) false) ; stub

(define ( search-for-file f n)
    (if (string=? n (file-name f))
         (file-data f)
         (search-for-listoffiles (file-subs f) n)))

(define ( search-for-listoffiles lof n)
    (cond [(empty? lof) false]
          [else
           (if (not (false?(search-for-file (first lof) n))) ; if the file is found
                (search-for-file (first lof) n)
                (search-for-listoffiles (rest lof) n))]))
```

### OptimizedSearchForFile
- Using `local` to avoid *recomputation* of the same values solves the performance issue in the previous implementation.
- The helper function `search-for-listoffiles` does not need to be visible to the rest of the program. This can be *encapsulated* using `local`, so that only `find-file` is visible to the rest of the program.
```racket
;; File String -> Integer or false
;; ListofFiles String -> Integer or false
;; Purpose: To find the file with the given name in the file system
(check-expect (find-file F2 "file1") false) ; "file1" is not present in F2
(check-expect (find-file F1 "file1") 1) ; "file1" is present in F1
(check-expect (find-file F6 "file1") 1) ; "file1" is present in F6

(local [(define ( search-for-file f n)
            (if (string=? n (file-name f))
                (file-data f)
                (search-for-listoffiles (file-subs f) n)))

        (define ( search-for-listoffiles lof n)
            (cond [(empty? lof) false]
                  [else
                    (local [define (search-for-file (first lof) n)]
                       (if (not (false? search)) ; if the file is found
                        search
                        (search-for-listoffiles (rest lof) n)))]))]
    (search-for-file f n))
```


---

