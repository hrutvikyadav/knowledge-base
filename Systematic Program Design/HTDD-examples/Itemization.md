---
id: Itemization
aliases:
  - Itemization
tags: []
---

# Itemization
[[Systematic Program Design#HTDD recipe|recipe]]
> [!tip] For #mixed-data-itemization; where each subclass is different type, use guard clauses against the types in the template.
> See example for [[Itemization#Countdown]]; refer to the Natural[1, 10] subclass in the cond clause.
> Also if we end up guarding for the same type in multiple clauses, and all other types are taken care of before that clauses, we can eliminate the guard clause for that type.

## Example problem statement

### Countdown
#data-definition
To design a system to manage a new years countdown, design a data definition to represent the current state of countdown in a program.
The state can be one of:
- NotStarted
- from 10 to 1 (seconds before new year)
- HappyNewYear

```racket
;; Countdown is one of:
;; - "NotStarted"
;; - Natural[1, 10]
;; - "HappyNewYear"
;; interp. as the current state of countdown where-
;; - "NotStarted" is the initial state
;; - Natural[1, 10] is the seconds remaining before new year
;; - "HappyNewYear" is the state when new year has arrived
(define CD1 "NotStarted")
(define CD2 10)            ; just started
(define CD2 3)             ; almost there
(define CD4 "HappyNewYear")

#;
(define (fn-for-countdown cd)
  (cond [(string=? cd "NotStarted") (...)]
        [(and (number? cd) (>= 1 cd) (<= 10 cd)) (... cd)]
        [(string=? cd "HappyNewYear") (...)]))
;; Template rules used:
;; - one of: 3 cases
;;   - atomic distinct: "NotStarted"
;;   - atomic non-distinct: Natural[1, 10]
;;   - atomic distinct: "HappyNewYear"

;; Simplified version - because the type definition is clear on what natural numbers are allowed
#;
(define (fn-for-countdown cd)
  (cond [(string=? cd "NotStarted") (...)]
        [(number? cd) (... cd)]
        [(string=? cd "HappyNewYear") (...)]))
```

### CountdownToNewYear
#htdf #non-primitive
Using the data definition for [[#Countdown|Countdown]] design a function that returns the current state of countdown.

```racket
;; Countdown -> Image
;; produce an image representing the current state of countdown
(check-expect (countdown-to-new-year "NotStarted") (empty-scene 100 100))
(check-expect (countdown-to-new-year 7) (text (number->string 7) 20 "black"))
(check-expect (countdown-to-new-year "HappyNewYear") (text "Happy New Year!" 20 "blue"))

#;
(define (countdown-to-new-year cd) (empty-scene 100 100)) ;stub

;; Template from Countdown
(define (countdown-to-new-year cd)
  (cond [(string=? cd "NotStarted") (empty-scene 100 100)]
        [(number? cd) (text (number->string cd) 20 "black")]
        [(string=? cd "HappyNewYear") (text "Happy New Year!" 20 "blue")]))
```

---
