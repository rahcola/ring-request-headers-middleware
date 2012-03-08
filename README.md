Ring middleware for parsing request headers.

It's common for many headers to have multiple values. Ring
catenates the values intoa single string separated by
commas. Wrap-request-headers allows you parse this string.

## ring.middleware.request-headers/wrap-request-headers

```clojure
(wrap-request-headers handler)
(wrap-request-headers handler parsing-rules)
```

The default behavior of wrap-request-headers is to split the header
value string into a sequence. So a request map looking like:

```clojure
{;...
 :headers {"accept" "text/html, application/xml; q=0.9"}
 ;...
}
```

will become:

```clojure
{;...
 :headers {"accept" ("text/html" "application/xml; q=0.9")}
 ;...
}
```

### Parsing

Many headers have their values augmented with extra
parameters. Probably the most common one is the "q" parameter for
denoting the "priority" of the value. Also in many cases the value is
better represented as some other Clojure data type than string. To
handle this, wrap-request-headers takes an optional map that contains
parsing functions.

The map should be a mapping from header name to parsing function to be
applied to every value of that header. If no parsing function is
provided for certain header the values are left untouched.

If the wrap-request-headers was applied with the following as
parsing-rules:

```clojure
{"accept" (fn [s] (let [[media-range & params] (.split s ";")]
                    (if (empty? params)
                        media-range
                        [media-range params])))}
```

the request map:

```clojure
{;...
 :headers {"accept" "text/html, application/xml; q=0.9"
           "accept-charset" "utf-8"}
 ;...
}
```

will become:

```clojure
{;...
 :headers {"accept" ("text/html" ["application/xml" "q=0.9"])
           "accept-charset" "utf-8"}
 ;...
}
```
