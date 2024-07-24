---
id: Abstraction
aliases:
  - Abstraction
tags: []
---

# Abstraction

## Fold Function
Design an abstract fold function for (listof X)
> [!note] here we want to design an abstract function directly from the template

> [!info] We can also design an abstract function from type comments; before that we want to practice designing abstract functions from the template i.e. follow the recipe backwards 

```racket
;; At this point in the course, the type (listof X) means:

;; ListOfX is one of:
;; - empty
;; - (cons X ListOfX)
;; interp. a list of X

;; and the template for (listof X) is:

(define (fn-for-lox lox)

  (cond [(empty? lox) (...)]
        [else
         (... (first lox)
              (fn-for-lox (rest lox)))]))
```

> [!hint] We know the fold function is a higher-order function that takes a function, an initial value, and a list of values. The function is applied to each element of the list, and result of the function is accumulated in the initial value.
> We add arguments to the template accordingly

```racket
;; (X X -> X) X (listof X) -> X
;; (X Y -> Y) Y (listof X) -> Y ???
;; Abstract fold function for (listof X)

(check-expect (fold + 0 (list 1 2 3)) 6
(check-expect (fold * 1 (list 1 2 3)) 6
(check-expect (fold string-append "" (list "a" "b" "c")) "abc")

(define (fold fn acc lox)
  (cond [(empty? lox) acc]
        [else
         (fn (first lox)
             (fold fn acc (rest lox)))]))
```
