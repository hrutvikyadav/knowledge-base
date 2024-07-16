---
id: Interval
aliases:
  - Interval
tags: []
---

# Interval
[[Systematic Program Design#HTDD recipe|recipe]]

## Example problem statement

### SeatNumber
#data-definition
You are designing a program to manage ticket sales for a theater that is perfectly rectangular shaped.
Design a data definition to represent the seat number in a single row(which has 32 seats).
> [!tip] The form of information is Interval in this case.

```racket
;; SeatNumber is Natural[1, 32] i.e. inclusive range from 1 to 32
;; interp. as a seat number in a row of a theater; 1 is the first seat and 32 is the last seat.
(define SN1 1)
(define SN2 32)
(define SN3 16)

#;
(define (fn-for-seat-number sn)
  (... sn))
;; (Data driven)Template rules used:
;; - atomic non-distinct: Natural[1, 32]
```

### AisleSeat
#htdf #non-primitive
Using the data definition for [[#SeatNumber|SeatNumber]] design a function that determines if a given seat number is an aisle seat.

```racket
;; SeatNumber -> Boolean
;; produce true if the given seat number is an aisle seat i.e. 1 or 32
(check-expect (aisle-seat? 1) true)
(check-expect (aisle-seat? 32) true)
(check-expect (aisle-seat? 16) false)
#;
(define (aisle-seat? sn) false) ;stub

;; Template from SeatNumber
(define (aisle-seat? sn)
  (or (= sn 1) (= sn 32)))
```

---
