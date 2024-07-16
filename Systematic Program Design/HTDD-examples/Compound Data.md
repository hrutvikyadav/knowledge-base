---
id: Compound Data
aliases:
  - Compound Data
tags: []
---

# Compound Data
[[Systematic Program Design#HTDD recipe|recipe]]

## Example problem statement

### Compound
#data-definition #simple-compound-data
Design a data definition to represent hockey players in a program. Each player has a first name, last name and number.

```racket
(define-struct player (first last number))
;; Player is (make-player String String Natural)
;; interp. a hockey player with first name, last name and number
(define P1 (make-player "Sidney" "Crosby" 87))
(define P2 (make-player "Connor" "McDavid" 69))

#;
(define (fn-for-player p)
  (... (player-first p)    ; String
       (player-last p)     ; String
       (player-number p))) ; Natural
;; Template rules used:
;; - compound: 3 fields
```

## Example problem statement involving world program

### CowABunga
#htdw #simple-compound-data
Design a world program with the following behaviour:
   - A cow walks back and forth across the screen.
   - When it gets to an edge it changes direction and goes back the other way
   - When you start the program it should be possible to control how fast a walker your cow is.
   - Pressing space makes it change direction right away.

#### Domain analysis
| Constants | Changing(`state`) | big-bang `api`'s |
| ------------- | -------------- | -------------- |
| screen-width, screen-height, cow y-axis position, background(mts), cow emoji | x-axis position of cow, direction of cow | `on-tick`, `to-draw`, `on-key` |

```racket
;; Constants
(define WIDTH 400)
(define HEIGHT 400)
(define CTR-Y (/ HEIGHT 2))
(define RCOW "🐄 ➡️")
(define LCOW "🐄 ⬅️")
(define MTS (empty-scene WIDTH HEIGHT))

;; Data definition
(define-struct cow (x xdir))
;; Cow is (make-cow Natural[0, WIDTH] Integer)
;; interp. a cow with x-axis position and direction of movement; direction if :
;; - +x moves right at x speed (pixels per tick)
;; - -x moves left at x speed (pixels per tick)
;; x is current x-axis position of cow
(define C1 (make-cow 0 1)) ;; cow currently at left edge moving right, 1px per tick
(define C2 (make-cow WIDTH -1)) ;; cow currently at right edge moving left, 1px per tick
#;
(define (fn-for-cow c)
  (... (cow-x c)      ; Natural[0, WIDTH]
       (cow-xdir c))) ; Integer
;; Template rules used:
;; - compound: 2 fields

;; Functions
;; Cow -> Cow
;; walk the cow, with (main C) where C is the cow
(define (main c)
    (big-bang c
            (on-tick next-cow)      ; Cow -> Cow
            (to-draw render-cow)    ; Cow -> Image
            (on-key handle-key)))   ; Cow KeyEvent -> Cow

;; Cow -> Cow
;; produce the next cow position; if cow is at edge, change direction
(check-expect (next-cow (make-cow 0 1)) (make-cow (+ 0 1) 1)     ; keeps moving normally
(check-expect (next-cow (make-cow 10 1)) (make-cow (+ 10 1) 1)   ; keeps moving normally
(check-expect (next-cow (make-cow 10 -1)) (make-cow (+ 10 -1) 1) ; keeps moving normally

(check-expect (next-cow (make-cow WIDTH 1)) (make-cow WIDTH -1)) ; special case, tries to cross right edge
(check-expect (next-cow (make-cow 0 -1)) (make-cow 0 1))    ; special case, tries to cross left edge

(check-expect (next-cow (make-cow 2 -3)) (make-cow 0 3)
#;
(define (next-cow C1) C1)

(define (next-cow c)
    (cond [(> (+ (cow-x c) (cow-xdir c)) WIDTH) (make-cow WIDTH (- (cow-xdir c)))]
          [(< (+ (cow-x c) (cow-xdir c)) 0)     (make-cow 0     (- (cow-xdir c)))]
          [else 
            (make-cow 
                (+ (cow-x c) (cow-xdir c))
                (cow-xdir c))])) 

;; Cow -> Image
;; produce an image of the cow at its current position
(check-expect (render-cow (make-cow 0 1)) (place-image RCOW 0 CTR-Y MTS)
(check-expect (render-cow (make-cow WIDTH -1)) (place-image LCOW WIDTH CTR-Y MTS)
#;
(define (render-cow C1) MTS)

(define (render-cow c)
  (place-image ( choose-image c) (cow-x c) CTR-Y MTS)) 

;; Cow -> Image
;; produce the image of cow based on its direction
(check-expect (choose-image (make-cow 0 1)) RCOW
(check-expect (choose-image (make-cow WIDTH -1)) LCOW
#;
(define (choose-image c) RCOW)

(define (choose-image c)
  (if (>= (cow-xdir c) 0) 
    RCOW
    LCOW))

;; Cow KeyEvent -> Cow
;; reverse x direction of cow when space is pressed
(check-expect (handle-key (make-cow 0 1) " ") (make-cow 0 -1)
(check-expect (handle-key (make-cow 0 -1) " ") (make-cow 0 1)
#;
(define (handle-key C1 " ") C1)

(define (handle-key c k)
  (cond [(string=? k " ") (make-cow (cow-x c) (- (cow-xdir c)))]
        [else c]))
```

---
