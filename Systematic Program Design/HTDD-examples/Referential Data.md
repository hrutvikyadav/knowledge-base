---
id: Referential Data
aliases:
  - Referential Data
tags: []
---

# Referential Data
[[Systematic Program Design#HTDD recipe|recipe]]

## Example Problem Statement

### SchoolList
#data-definition #referential-data #referential-non-primitive-data
Design a data definition to represent a list of schools.
```racket
(define-struct school (name tuition))
;; School is (make-school String Number)
;; interp. a school with its name and tuition
(define S1 (make-school "School1" 10000))
(define S2 (make-school "School2" 20000))
(define S3 (make-school "School3" 30000))
#;
(define (fn-for-school s)
  (... (school-name s) ; String
       (school-tuition s))) ; Number
;; Template rules used:
;; - compound: 2 fields

;; ListOfSchool is one of:
;; - empty
;; - (cons School ListOfSchool)
(define LOS1 empty)
(define LOS2 (cons S1 empty))
(define LOS3 (cons S1 (cons S2 empty)))
#;
(define (fn-for-los los)
  (cond [(empty? los) (...)]
        [else
         (... (fn-for-school (first los))
              (fn-for-los (rest los)))]))
;; Template rules used:
;; - one of: 2 cases
;; - atomic distinct: empty
;; - compound: (cons School ListOfSchool)
;; - reference: (first los) is School
;; - self-reference: (rest los) is ListOfSchool
```

### SchoolTutionGraph
#htdf #non-primitive-data
Design a function to represent a graph of schools and their tuition.
```racket
;; ListOfSchool -> Image
;; produce a graph of schools and their tuition
(check-expect (school-tuition-graph empty) (empty-scene 0 0))
(check-expect (school-tuition-graph (cons S1 empty))
              (beside/align "bottom"
                (empty-scene 0 0)
                (overlay/align
                    "bottom"
                    (text "School1" FONT-SIZE FONT-COLOR)
                    (rectangle BAR-WIDTH (* 10000 Y-SCALE) "solid" BAR-COLOR)
                    (rectangle BAR-WIDTH (* 10000 Y-SCALE) "outline" "black"))))
(check-expect (school-tuition-graph (cons S1(cons S2 empty)))
              (beside/align "bottom"
                (empty-scene 0 0)
                (overlay/align
                    "bottom"
                    (text "School1" FONT-SIZE FONT-COLOR)
                    (rectangle BAR-WIDTH (* 10000 Y-SCALE) "solid" BAR-COLOR)
                    (rectangle BAR-WIDTH (* 10000 Y-SCALE) "outline" "black"))
                (overlay/align
                    "bottom"
                    (text "School2" FONT-SIZE FONT-COLOR)
                    (rectangle BAR-WIDTH (* 10000 Y-SCALE) "solid" BAR-COLOR)
                    (rectangle BAR-WIDTH (* 10000 Y-SCALE) "outline" "black"))))
#;
(define (school-tuition-graph los) (empty-scene 0 0)) ; stub

(define (school-tuition-graph los)
  (cond [(empty? los) (empty-scene 0 0)]
        [else
         (beside/align
            "bottom"
            (school-bar (first los))
            (fn-for-los (rest los)))]))

;; School -> Image
;; produce a bar graph of school and its tuition
(check-expect (school-bar S1)
              (overlay/align
                "bottom"
                (text "School1" FONT-SIZE FONT-COLOR)
                (rectangle BAR-WIDTH (* 10000 Y-SCALE) "solid" BAR-COLOR)
                (rectangle BAR-WIDTH (* 10000 Y-SCALE) "outline" "black")))
#;
(define (school-bar s) (empty-scene 0 0)) ; stub

(define (school-bar s)
  (overlay-align
    "bottom"
    (text (school-name s) FONT-SIZE FONT-COLOR)
    (rectangle BAR-WIDTH (* (school-tuition s) Y-SCALE ) "solid" BAR-COLOR)
    (rectangle BAR-WIDTH (* (school-tuition s) Y-SCALE ) "outline" BAR-COLOR))) 
```

---
