## Chapter 9 - Build and Deployment JS App

### 1. Apache ANT

### 2. Combining JS files

### 3. Preprocessing JavaScript Files

In computer science, a preprocessor is a program that processes its input data to produce output that is used as input to another program. The output is said to be a preprocessed form of the input data, which is often used by some subsequent programs like compilers. The amount and kind of processing done depends on the nature of the preprocessor; some preprocessors are only capable of performing relatively simple textual substitutions and macro expansions, while others have the power of fully fledged programming languages.

    — http://en.wikipedia.org/wiki/Preprocessor

### 4. JavaScript Minification

For example:

- Replacement of bracket notation with dot notation whenever possible (e.g., `foo["bar"]` becomes `foo.bar`)
- Replacement of quoted literal property names whenever possible (e.g., `{"foo":"bar"}` becomes `{foo:"bar"}`)
- Replacement of escaped quotes in strings (e.g., `'aaa\'bbb'` becomes `"aaa'bbb"`)
- Constant folding (e.g., `"foo"+"bar"` becomes `"foobar"`)

### 5. Buildtime Versus Runtime Build Processes

As a general rule for building high-performance applications, everything that can be done at buildtime should not be done at runtime.

### 6. JS Compression

#### 6.1 Client side http header

When a web browser requests a resource, it usually sends an `Accept-Encoding`([mdn do](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding)c) HTTP header (starting with HTTP/1.1) to let the web server know what kinds of encoding transformations it supports. Possible values for the Accept-Encoding value tokens include: **gzip**, compress, deflate, and identity (these values are registered by the Internet Assigned Numbers Authority, or IANA).

```
Accept-Encoding: gzip
Accept-Encoding: compress
Accept-Encoding: deflate
Accept-Encoding: br
Accept-Encoding: identity
Accept-Encoding: *

// Multiple algorithms, weighted with the [quality value](https://developer.mozilla.org/en-US/docs/Glossary/Quality_values) syntax:
Accept-Encoding: deflate, gzip;q=1.0, *;q=0.5
```

#### 6.2 Server side http header

If the web server sees this header in the request, it will choose the most appropriate encoding method and notify the web browser of its decision via the `Content-Encoding` ([mdn doc](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding))HTTP header.

```
Content-Encoding: gzip
Content-Encoding: compress
Content-Encoding: deflate
Content-Encoding: br

// Multiple, in the order in which they were applied
Content-Encoding: deflate, gzip
```

### 7. Caching JS Files

Web servers use the `Expires` HTTP **response header** to let clients know how long a resource can be cached.

The format is an absolute timestamp in RFC 1123 format. An example of its use is: **Expires: Thu, 01 Dec 1994 16:00:00 GMT.**

To mark a response as **“never expires,”** a web server sends an `Expires` date approximately **one year** in the future from the time at which the response is sent. Web servers should **never** send Expires dates **more than one year** in the future according to the HTTP 1.1 RFC (RFC 2616, section 14.21).

### 8. Using a Content Delivery Network

A content delivery network (CDN) is a network of computers distributed geographically across the Internet that is responsible for delivering content to end users.

The primary reasons for using a CDN are **reliability, scalability**, and above all, **performance**. In fact, by serving content from the location closest to the user, CDNs are able to **dramatically decrease network latency**.

### 9. Summary

- Combining JavaScript files to reduce the number of HTTP requests
- Minifying JavaScript files using the YUI Compressor
- Serving JavaScript files compressed (gzip encoding)
- Making JavaScript files cacheable by setting the appropriate **HTTP response headers** and work around caching issues by appending a timestamp to filenames
- Using a Content Delivery Network to serve JavaScript files; not only will a **CDN improve performance**, it should also manage compression and caching for you## Chapter 9 - Build and Deployment JS App

### 1. Apache ANT

### 2. Combining JS files

### 3. Preprocessing JavaScript Files

In computer science, a preprocessor is a program that processes its input data to produce output that is used as input to another program. The output is said to be a preprocessed form of the input data, which is often used by some subsequent programs like compilers. The amount and kind of processing done depends on the nature of the preprocessor; some preprocessors are only capable of performing relatively simple textual substitutions and macro expansions, while others have the power of fully fledged programming languages.

    — http://en.wikipedia.org/wiki/Preprocessor

### 4. JavaScript Minification

For example:

- Replacement of bracket notation with dot notation whenever possible (e.g., `foo["bar"]` becomes `foo.bar`)
- Replacement of quoted literal property names whenever possible (e.g., `{"foo":"bar"}` becomes `{foo:"bar"}`)
- Replacement of escaped quotes in strings (e.g., `'aaa\'bbb'` becomes `"aaa'bbb"`)
- Constant folding (e.g., `"foo"+"bar"` becomes `"foobar"`)

### 5. Buildtime Versus Runtime Build Processes

As a general rule for building high-performance applications, everything that can be done at buildtime should not be done at runtime.

### 6. JS Compression

#### 6.1 Client side http header

When a web browser requests a resource, it usually sends an `Accept-Encoding`([mdn do](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Encoding)c) HTTP header (starting with HTTP/1.1) to let the web server know what kinds of encoding transformations it supports. Possible values for the Accept-Encoding value tokens include: **gzip**, compress, deflate, and identity (these values are registered by the Internet Assigned Numbers Authority, or IANA).

```
Accept-Encoding: gzip
Accept-Encoding: compress
Accept-Encoding: deflate
Accept-Encoding: br
Accept-Encoding: identity
Accept-Encoding: *

// Multiple algorithms, weighted with the [quality value](https://developer.mozilla.org/en-US/docs/Glossary/Quality_values) syntax:
Accept-Encoding: deflate, gzip;q=1.0, *;q=0.5
```

#### 6.2 Server side http header

If the web server sees this header in the request, it will choose the most appropriate encoding method and notify the web browser of its decision via the `Content-Encoding` ([mdn doc](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Encoding))HTTP header.

```
Content-Encoding: gzip
Content-Encoding: compress
Content-Encoding: deflate
Content-Encoding: br

// Multiple, in the order in which they were applied
Content-Encoding: deflate, gzip
```

### 7. Caching JS Files

Web servers use the `Expires` HTTP **response header** to let clients know how long a resource can be cached.

The format is an absolute timestamp in RFC 1123 format. An example of its use is: **Expires: Thu, 01 Dec 1994 16:00:00 GMT.**

To mark a response as **“never expires,”** a web server sends an `Expires` date approximately **one year** in the future from the time at which the response is sent. Web servers should **never** send Expires dates **more than one year** in the future according to the HTTP 1.1 RFC (RFC 2616, section 14.21).

### 8. Using a Content Delivery Network

A content delivery network (CDN) is a network of computers distributed geographically across the Internet that is responsible for delivering content to end users.

The primary reasons for using a CDN are **reliability, scalability**, and above all, **performance**. In fact, by serving content from the location closest to the user, CDNs are able to **dramatically decrease network latency**.

### 9. Summary

- Combining JavaScript files to reduce the number of HTTP requests
- Minifying JavaScript files using the YUI Compressor
- Serving JavaScript files compressed (gzip encoding)
- Making JavaScript files cacheable by setting the appropriate **HTTP response headers** and work around caching issues by appending a timestamp to filenames
- Using a Content Delivery Network to serve JavaScript files; not only will a **CDN improve performance**, it should also manage compression and caching for you
