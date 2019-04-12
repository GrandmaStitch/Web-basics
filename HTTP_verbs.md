*** Web Server

  - a piece of software designed to accept incoming web requests

*** HTTP request

  - **HTTP Request** is a packet of Information that one computer sends to another computer to communicate something. To its core, HTTP Request is a packet of binary data sent by the Client to server. An HTTP Request contains three parts: request line, headers and body.

  - Request line: A Request Line specifies the Method Token (GET, PUT … ) followed by the Request URI and then the HTTP Protocol that is being used

  - Headers, 0 or more Headers in the request: Headers are used to pass additional information about the request to the server. In the request section, whatever follows Request Line till before Request Body everything is a Header.

  - An optional Body of the request: Request Body is the part of the HTTP Request where additional content can be sent to the server.

  **** A GET request

    `https://www.google.com/`

    - request line: GET / HTTP/1.1

      - GET: Request method type

      - /: Path

      - HTTP/1.1: HTTP protocol version

    `https://docs.python.org/3/tutorial/stdlib.html#string-pattern-matching`

    - request line: GET /3/tutorial/stdlib.html HTTP/1.0

    - host: docs.python.org

    - fragment: #string-pattern-matching

https://www.toolsqa.com/rest-assured/what-is-rest/

    