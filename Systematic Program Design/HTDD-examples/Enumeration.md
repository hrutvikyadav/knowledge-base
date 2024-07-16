---
id: Enumeration
aliases:
  - Enumeration
tags: []
---

# Enumeration
[[Systematic Program Design#HTDD recipe|recipe]]
> [!tip] Enumeration fields/subclasses are (atomic)distinct and fixed in number.
> If one of the fields is non-distinct, use [[Itemization]].

## Example problem statement

### LetterGrade
#data-definition
To design a system to manage student grades, you need to represent the letter grade of a student. Grades can be one of A, B, C.

```racket
;; LetterGrade is one of:
;; - "A"
;; - "B"
;; - "C"
;; interp. as a letter grade of a student
;; examples are redundant for enumerations

#;
(define ( fn-for-letter-grade lg )
    (cond [(string=? lg "A") (...)]
          [(string=? lg "B") (...)]
          [(string=? lg "C") (...)]))
;; Template rules used:
;; - one of: 3 cases
;;   - atomic distinct: "A"
;;   - atomic distinct: "B"
;;   - atomic distinct: "C"
```

### BumpUp
#htdf #non-primitive
Using the data definition for [[#LetterGrade|LetterGrade]] design a function that returns the next higher grade.

```racket
;; LetterGrade -> LetterGrade
;; produce the next higher grade of the given letter grade; A simply returns A
(check-expect (bump-up "A") "A")
(check-expect (bump-up "B") "A")
(check-expect (bump-up "C") "B")
#;
(define (bump-up lg) "A") ;stub

;; Template from LetterGrade
(define (bump-up lg)
  (cond [(string=? lg "A") "A"]
        [(string=? lg "B") "A"]
        [(string=? lg "C") "B"]))
```

---
