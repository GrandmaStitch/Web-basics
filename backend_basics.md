### Static content and more

  - The Web was originally designed to serve documents, not to deliver applications. Even today, a large amount of the data presented on any web site is static content — images, HTML files, videos, downloadable files, and other media stored on disk.

  - Specialized web server programs — like **[Apache](https://httpd.apache.org/)**, **[Nginx](https://www.nginx.com/resources/wiki/)**, or **[IIS](https://www.iis.net/)** — can serve static content from disk storage very quickly and efficiently. They can also provide access control, allowing only authenticated users to download particular static content.

&nbsp;

### Routing and load balancing

  - Some web applications have several different server components, each running as a separate process. One thing *a specialized web server* can do is **dispatch requests to the particular backend servers that need to handle each request**. There are a lot of names for this, including **request routing** and **reverse proxying**.

  - Some web applications need to do a lot of work on the server side for each request, and need many servers to handle the load. **Splitting requests up among several servers is called load balancing**.

  - Load balancing also helps **handle conditions where one server becomes unavailable, allowing other servers to pick up the slack**. A reverse proxy can **health check the backend servers, only sending requests to the ones that are currently up and running**. This also makes it possible to do updates to the backend servers without having an outage.

&nbsp;

### Concurrent users

  - Handling a large number of network connections at once turns out to be complicated — even more so than plugging concurrency support into your Python web service.

  - As you may have noticed in your own use of the web, it takes time for a server to respond to a request. The server has to receive and parse the request, come up with the data that it needs to respond, and transmit the response back to the client. The network itself is not instantaneous; it takes time for data to travel from the client to the server. In addition, a browser is totally allowed to open up multiple connections to the same server, for instance to request resources such as images, or to perform API queries.

  - All of this means that if a server is handling many requests per second, there will be many requests in progress at once — literally, at any instant in time. We sometimes refer to these as in-flight requests, meaning that the request has "taken off" from the client, but the response has not "landed" again back at the client. A web service can't just handle one request at a time and then go on to the next one; it has to be able to handle many at once.

&nbsp;

### Caching

  - Imagine a web service that does a lot of complicated processing for each request — something like calculating the best route for a trip between two cities on a map. Pretty often, users make the same request repeatedly: imagine if you load up that map, and then you reload the page — or if someone else loads the same map. It's useful if the service can avoid recalculating something it just figured out a second ago. It's also useful if the service can avoid re-sending a large object (such as an image) if it doesn't have to.

  - One way that web services avoid this is by making use of a cache, **a temporary storage for resources that are likely to be reused**. Web systems can perform caching in a number of places — but all of them are under control of the server that serves up a particular resource. That server can set HTTP headers indicating that a particular resource is not intended to change quickly, and can safely be cached.

  - There are a few places that caching usually can happen. Every user's browser maintains **a browser cache of cacheable resources** — such as images from recently-viewed web pages. The browser can also be configured to pass requests through **a web proxy**, which can perform caching on behalf of many users. Finally, a web site can use **a reverse proxy** to cache results so they don't need to be recomputed by a slower application server or database. **All HTTP caching is supposed to be governed by cache control headers set by the server**. You can read a lot more about them in [this article](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching) by Google engineer Ilya Grigorik.

&nbsp;

### Capacity

  - Why serve static requests out of cache (or a static web server) rather than out of your application server? Python code is totally capable of sending images or video via HTTP, after all. The reason is that — all else being equal — handling a request faster provides a better user experience, but also makes it possible for your service to support more requests.

  - If your web service becomes popular, you don't want it to bog down under the strain of more traffic. So it helps to handle different kinds of request with software that can perform that function quickly and efficiently.

&nbsp;

### Cookies

  - Cookies are a way that **a server can ask a browser to retain a piece of information, and send it back** to the server when the browser makes subsequent requests. Every cookie has a name and a value, much like a variable in your code; it also has **rules that specify** when the cookie should be sent back.

  - What are cookies for? A few different things. If the server sends each client a unique cookie value, it can use these to tell clients apart. This can be used to implement higher-level concepts on top of HTTP requests and responses — things like sessions and login. Cookies are used by **analytics and advertising systems to track user activity from site to site**. Cookies are also sometimes used to **store user preferences for a site**.

  #### How cookies happen

  - The first time the client makes a request to the server, the server sends back the response with a **Set-Cookie header**. This header contains three things: *a cookie name*, *a value*, and *some attributes*. Every subsequent time the browser makes a request to the server, it will send that cookie back to the server. **The server can update cookies, or ask the browser to expire them**.

  #### Seeing cookies in your browser

  - Browsers don't make it easy to find cookies that have been set, because removing or altering cookies can affect the expected behavior of web services you use. However, it is possible to inspect cookies from sites you use in every major browser. View more details, read [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) and [Set-Cookie-HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie).

  ![cookies in browser](imgs/CookiesInBrowser.png)

  - The first two, the cookie's name and content, are also called its **key and value**. They're analogous to a dictionary key and value in Python — or a variable's name and value for that matter. They will both be sent back to the server. There are some syntactic rules for which characters are allowed in a cookie name; for instance, they can't have spaces in them. The value of the cookie is where **the "real data" of the cookie goes** — for instance, a unique token representing a logged-in user's session.

  - The next two fields, Domain and Path, describe the scope of the cookie — that is to say, which queries will include it. By default, the domain of a cookie is the hostname from the URI of the response that set the cookie. But a server can also set a cookie on a broader domain, within limits. For instance, a response from www.udacity.com can set a cookie for udacity.com, but not for com.

  - The fields that Chrome describes as "Send for" and "Accessible to script" are internally called **Secure and HttpOnly**, and they are boolean flags (true or false values). The internal names are a little bit misleading. **If the Secure flag is set, then the cookie will only be sent over HTTPS (encrypted) connections, not plain HTTP. If the HttpOnly flag is set, then the cookie will not be accessible to JavaScript code running on the page**.

  - Finally, the last two fields deal with the lifetime of the cookie — how long it should last. The creation time is just the time of the response that set the cookie. The expiration time is when the server wants the browser to stop saving the cookie. There are two different ways a server can set this: it can set an Expires field with a specific date and time, or a Max-Age field with a number of seconds. **If no expiration field is set, then a cookie is expired when the browser closes**.

&nbsp;

### DNS domains and cookie security

  - Domain names play a few other roles in HTTP besides just being easier to remember than IP addresses. A DNS domain links a particular hostname to a computer's IP address. But it also indicates that the owner of that domain intends for that computer to be treated as part of that domain.

  - Imagine what a bad guy could do if they could convince your browser that their server *evilbox* was part of (say) *Facebook*, and get you to request *a Facebook URL* from *evilbox* instead of from *Facebook's real servers*. Your browser would send your *facebook.com* cookies to *evilbox* along with that request. But these cookies are what prove your identity to Facebook … so then the bad guy could use those cookies to access your Facebook account and send spam messages to all your friends.

  - This is just one reason that DNS is essential to web security. If a bad guy can take control of your site's DNS domain, they can send all your web traffic to their evil server … and if the bad guy can fool users' browsers into sending that traffic their way, they can steal the users' cookies and reuse them to break into those users' accounts on your site.

&nbsp;

### HTTPS

  - When a browser and a server speak HTTPS, they're just speaking HTTP, but over an encrypted connection. The encryption follows a standard protocol called **Transport Layer Security**, or TLS for short. TLS provides some important guarantees for web security:

    - It keeps the connection private by encrypting everything sent over it. Only the server and browser should be able to read what's being sent.
    - It lets the browser authenticate the server. For instance, when a user accesses https://www.udacity.com/, they can be sure that the response they're seeing is really from Udacity's servers and not from an impostor.
    - It helps protect the integrity of the data sent over that connection — checking that it has not been (accidentally or deliberately) modified or replaced.
    - TLS is also very often referred to by the older name SSL (Secure Sockets Layer). Technically, SSL is an older version of the encryption protocol.

  #### Keys and certificates

    - The server-side configuration for TLS includes two important pieces of data: **a private key** and **a public certificate**. The private key is secret; it's held on the server and never leaves there. The certificate is sent to every browser that connects to that server via TLS. These two pieces of data are mathematically related to each other in a way that makes the encryption of TLS possible.
    - The server's certificate is issued by an organization called a **certificate authority** (CA). The certificate authority's job is to make sure that the server really is who it says it is — for instance, that a certificate issued in the name of Heroku is actually being used by the Heroku organization and not by someone else.
    - The role of a certificate authority is kind of like getting a document notarized. A notary public checks your ID and witnesses you sign a document, and puts their stamp on it to indicate that they did so.

  #### How does TLS assure privacy?

    - The data in the TLS certificate and the server's private key are mathematically related to each other through a system called [public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography). The important part is that the two endpoints (the browser and server) can securely agree on a shared secret which allows them to scramble the data sent between them so that only the other endpoint — and not any eavesdropper — can unscramble it.

  #### How does TLS assure authentication?

    - A server certificate indicates that an encryption key belongs to a particular organization responsible for that service. It's the job of a certificate authority to make sure that they don't issue a cert for (say) udacity.com to someone other than the company who actually runs that domain. But the cert also contains metadata that says what DNS domain the certificate is good for. When the browser connects to a particular server, if the TLS domain metadata doesn't match the DNS domain, the browser will reject the certificate and put up a big scary warning to tell the user that something fishy is going on.

  #### How does TLS assure integrity?

    - Every request and response sent over a TLS connection is sent with a [message authentication code](https://en.wikipedia.org/wiki/Message_authentication_code) (MAC) that the other end of the connection can verify to make sure that the message hasn't been altered or damaged in transit.


