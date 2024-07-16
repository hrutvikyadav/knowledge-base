---
id: Self Referential Data
aliases:
  - Self Referential Data
tags: []
---

# Self Referential Data
[[Systematic Program Design#HTDD recipe|recipe]]

## Example Problem Statement

### IPL Teams
#data-definition #self-referential-data
Design a data definition to represent list of IPL teams.
```racket
;; A ListOfIPLTeams is one of:
;; - empty
;; - (cons IPLTeam ListOfIPLTeams) where IPLTeam is a string
(define LOIT1 empty)
(define LOIT2 (cons "CSK" empty))
(define LOIT3 (cons "CSK" (cons "MI" empty)))
#;
(define ( fn-for-loit loit)
    (cond [(empty? loit) (...)]
          [else
           (... (first loit)                  ; String
                (fn-for-loit (rest loit)))])) ; ListOfIPLTeams
;; Template rules used:
;; - one of: 2 cases
;; - atomic distinct: empty
;; - compound: (cons String ListOfIPLTeams)
;; - self-reference: (rest loit) is ListOfIPLTeams
```

### Favorite IPL Team
#htdf #non-primitive-data
Design a function to determine if MI IPL team exists in the list of IPL teams.
```racket
;; ListOfIPLTeams -> Boolean
;; produce true if MI exists in the list of IPL teams
(check-expect (favorite-ipl-team empty) false)
(check-expect (favorite-ipl-team (cons "CSK" empty)) false)
(check-expect (favorite-ipl-team (cons "CSK" (cons "MI" empty))) true)
#;
(define (favorite-ipl-team loit) false) ; stub

(define ( favorite-ipl-team loit)
    (cond [(empty? loit) false]
          [else
           (if (string=? (first loit) "MI")                  ; String
                true
                (favorite-ipl-team (rest loit)))])) ; ListOfIPLTeams
```

---
