---
id: Atomic Non Distinct
aliases:
  - Atomic Non Distinct
tags: []
---

# Atomic Non Distinct
[[Systematic Program Design#HTDD recipe|recipe]]

## Example problem statement

### CityName
#data-definition
The problem domain has information about city names; Design a data definition to represent city names in a program.

| Information(Problem domain) | Data(Program) |
| ------------- | -------------- |
| Pune | "Pune" |
| Mumbai | "Mumbai" |
| Bangalore | "Bangalore" |

```racket
;; CityName is String
;; interp. as a city name in India
(define CN1 "Pune")
(define CN2 "Mumbai")
(define CN3 "Bangalore")

#;
(define (fn-for-city-name cn)
  (... cn))
;; (Data driven)Template rules used:
;; - atomic non-distinct: String
```

### BestCity
#htdf #non-primitive
Using the [[Atomic Non Distinct#cityname|CityName data definition]], design a function which takes a cityname and returns boolean to determine if it is the best city in India.

```racket
;; CityName -> Boolean
;; produce true if the city is(Pune) the best city in India
(check-expect (best-city? "Pune") true)
(check-expect (best-city? "Mumbai") false)
(check-expect (best-city? "Bangalore") false)

;; (define (best-city? cn) false) ;stub

;; Template from `fn-for-city-name`
(define (best-city? cn)
  (cond [(string=? cn "Pune") true]
        [else false]))

;; Simplified version
#;
(define (best-city? cn)
  (string=? cn "Pune"))
```

---

