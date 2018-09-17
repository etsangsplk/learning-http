# HTTP MVP

- There are multiple versions of HTTP
- A HTTP 1.0/1.1 request is nothing more than a few lines of text. (We will focus on 1.0 b/c it is simpler)
- HTTP 2.0 is a binary format (We will not touch on this)
- For each HTTP request, there is a single HTTP response
- The structure of an HTTP 1.0 request and response is defined by [RFC 1945](https://tools.ietf.org/html/rfc1945)
- [Wikipedia](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) has a good example
- Each line is seperated by a Carraige Return byte (CR = "\r") followed by a Line Feed byte (LF = "\n")

# Request Structure

| Request Line    | "GET /abc HTTP/1.1"     | "<method> <path> <protocol>" |
| Headers         | "Host: www.example.com" | "<key>: <value>"             |
| Empty Line      | ""                      | ""                           |
| Body (Optional) | "Hello!"                | N/A                          |

We can parse this structure using the following logic:

With a line determined by: everything read up to the CRLF ("\r\n")...

1. Split the first line of the request on " ":

```
split[0] = Method
split[1] = Path
split[2] = Protocol
```

2. For each line following (that isnt empty), split on ":" a maximum of 2 times:

```
split[0] = Header key
split[1] = Header value

```

Note: HTTP allows for duplicates of the same header keys. This is why Go stores headers as: `map[string][]string` rather than `map[string]string`.

3. When a empty line is encountered, we know we have reached the end of the headers.

4. Body: TODO
