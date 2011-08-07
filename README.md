Ring middleware for parsing request headers.

Default behavior turns header values like
```"text/html, application/xml" ```
into
```("text/html" "application/xml") ```
