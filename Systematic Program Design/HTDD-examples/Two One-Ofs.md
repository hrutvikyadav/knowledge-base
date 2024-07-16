---
id: Two One-Ofs
aliases:
  - Two One-Ofs
tags: []
---

# Two One-Ofs

Here we learn to design functions consuming ==two arguments that have one-of types==. In order *to do this, we will develop a new model* of our code, the *cross product of the types comment table*.
> The cross product of the types comment table first provides us with a way to **clearly think of all possible test cases**. We will then find it *helps us get a good idea of what our function's body* will look like -- and even allow us to *simplify it* -- before we start coding. 
> In this sense, it plays a role similar to the role of type comments and templates -- 
> and in fact extends our idea of templating from simply what we copy from the types comments to what it really is; a method of knowing a great deal about what code will look like before we start coding details.

## Example Problem Statement
#two-one-ofs #cross-product-of-types

Design a function that consumes two list of strings and produces true if listA is prefix of listB, otherwise false.

### Data Definitions
```racket
;; ListOfString is one of:
;; - empty
;; - (cons String ListOfString)
;; interp. a list of strings

(define LS0 empty)
(define LS1 (cons "a" empty))
(define LS2 (cons "a" (cons "b" empty)))
(define LS3 (cons "c" (cons "b" (cons "a" empty))))

#;
(define (fn-for-los los)
  (cond [(empty? los) (...)]
        [else 
         (... (first los)
              (fn-for-los (rest los)))]))
```

### Cross Product of Types

| lstb👇lsta👉 | empty | (cons String ListOfString) |
| ------------- | -------------- | -------------- |
| empty | Both lists are empty *(ans: true)* | lstb is empty ; lsta has some elements *(ans: false)* |
| (cons String ListOfString) | lsta is empty ; lstb has some elements *(ans: true)* | both lists have some elements *(if first elems are = and NR return true)* |
> We need atleast these 4 test cases(`examples`) to cover all possible scenarios.
> More test cases can be added to cover edge cases.

> Also while templating we cannot use the templates of the types involved, as they don't make sense in this case. 
> We will use the cross product of the types comment table(i.e. 4 cases minimum) to `template` the function.

> Where answers in table are same; we can usually cosider that as one case.

### Functions
```racket

;; LOS, LOS -> Boolean
;; produce true if list 1 is a prefix of list 2.

;; 4 examples from param cross product table, and more
(check-expect (prefix=? empty empty) true)
(check-expect (prefix=? (list "X") empty) false)
(check-expect (prefix=? empty (list "X")) true)
(check-expect (prefix=? (list "X") (list "X")) true)

(check-expect (prefix=? (list "X" "Y") (list "X")) false)
(check-expect (prefix=? (list "X") (list "X" "Y")) true)
(check-expect (prefix=? (list "X" "Y") (list "X" "Y")) true)

; The cross product table tells us there are 4 cases to consider; but looking at the examples, we can simplify the template even more
#;
(define (prefix=? lsta lstb)
    (cond [(and (empty? lsta) (empty? lstb) ( ... ))]
          [(and (cons? lsta) (empty? lstb)) (... lsta ...)]
          [(and (cons? lstb) (empty? lsta)) (... lstb ...)]
          [(and (cons? lstb) (cons? lsta)) (... lsta lstb ...)]))

(define (prefix=? lsta lstb)
    (cond
      [(empty? lsta) true]
      [(empty? lstb) false]
      [(and (string=? (first lsta) (first lstb))
            (prefix=? (rest lsta) (rest lstb)))])))
```

---
