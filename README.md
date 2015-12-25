# csv

A minimal CSV library in and for zepto.
It is largely compatible with Clojure's data.csv.

## Usage

This library contains the functions `read` and `write`:

````clojure
(load "csv/csv")
(define csv:read (import "csv:read"))
(define csv:write (import "csv:write"))

; let's read something in
(csv:read "a,b\n1,2\n3,4") ; => (("a" "b") ("1" "2") ("3" "4"))

; quoting also works
(csv:read "a,b\n1,\"2,2\"\n3,4") ; => (("a" "b") ("1" "2,2") ("3" "4"))

; you can also read from a file; if the first argument is a port,
; it will automatically read its contents
(csv:read (open-input-port "foo.csv")) ; => <the parsed contents of foo.csv>

; need a custom separator? gotcha.
(csv:read "a-b\n1-\"2,2\"\n3-4" #{:separator #\-}) ; => (("a" "b") ("1" "2,2") ("3" "4"))

; now for the other direction:
(csv:write [("a" "b") ("1" "22") ("3" "4")]) ; => "a,b\n1,22\n3,4"

; custom separators? okidok.
(csv:write [("a" "b") ("1" "22") ("3" "4")] #{:separator #\%}) ; => "a%b\n1%22\n3%4"

; need to write somewhere?
(csv:write [("a" "b") ("1" "22") ("3" "4")] #{:separator #\% :port :stdout}) ; => <will write "a%b\n1%22\n3%4" to stdout>
; of course any other port also works.
```

A complete list of modifiers and their default values:

```clojure
; for write:
#{:separator #\, ; will separate cells
  :quote #\" ; will quote cells
  :port #f ; if set, write to there
  :quote? #t}) ; should we even quote, even if it would make sense?

; for read:
#{:separator #\, ; the cell separator
  :quote #\"}) ; the cell quoting char
```

<hr/>
Have fun!
