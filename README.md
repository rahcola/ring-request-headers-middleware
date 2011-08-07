Ring middleware for parsing request headers.

Default behavior turns header values like
```clojure
"text/html, application/xml"
```
into
```clojure
("text/html" "application/xml")
```
