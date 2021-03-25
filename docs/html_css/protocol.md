## Web Network Protocol Comparison

#### I. [network performance: http1.0  vs http2](#question-1)

#### II. [network choices for Loading new Feeds](#question-2)

#### III. [How CDN helps performance?](#question-3)

<div id="question-1"/>

### I. Network Performance: http1.0  vs http2

HTTP1 Disadvantages:
- HTTP/1.1 practically allows only one outstanding request per TCP connection
	even though browser implements multiple parallel TCP connections to every domain.
- **Duplication of data across requests (cookies and other headers).** 
	Too many requests means too much redundant data, which would impact performance.

**HTTP2 Advantages:** 
reference article: [link](https://imagekit.io/blog/http2-vs-http1-performance/)
- **Multiplexed, instead of ordered**
	Allows using same TCP connection for multiple parallel requests. **Eg: images, js files starts to download in parallel, very quick.**
- ****Header compression using HPACK****
	Compressed headers, reduced data redundancy
- ****Server Push****
	Instead of waiting for the client to request for assets like JS and CSS, the server can “push” the resources it believes would be required by the client. Avoids the round trip.

<div id="question-2"/>

### II. network choices for Loading new Feeds
- traditional Polling: with time interval
- Long Polling
- WebSockets
- SSE: Server Send Event

#### 2.1 Long polling
This is a variation of the traditional polling technique that allows the server to push information to a client whenever the data is available.
- 1 ) The client makes an initial request using regular HTTP and then waits for a response.
- 2 ) The server delays its response until an update is available or a timeout has occurred.
- 3 )  When an update is available, the server sends a full response to the client.
- 4 ) The client typically sends a new long-poll request, either immediately upon receiving a response or after a pause to allow an acceptable latency period.
- 5 ) Each Long-Poll request has a timeout. The client has to reconnect periodically after the connection is closed due to timeouts.

#### 2.2 Websocket
- 1 ) provides [Full duplex](https://en.wikipedia.org/wiki/Duplex_(telecommunications)#Full_duplex) communication channels over a single TCP connection. 
- 2 ) It provides a persistent connection between a client and a server that both parties can use to start sending data at any time.

#### 2.3 Server Send Event
- 1 )  Client requests data from a server using regular HTTP.
- 2 ) The requested webpage opens a connection to the server.
- 3 )  The server sends the data to the client whenever there’s new information available.

**Advantage:** 
SSEs are best when we need **real-time traffic** from the server to the client or if the server is generating data in a loop and will be sending multiple events to the client.

**Disadvantage:**
BUT: If the **~~client wants to send data to the server,~~** it would require the use of **another technology/protocol** to do so.

<div id="question-3"/>

### III. How CDN helps performance?
- CDN has a quick time to fetch static resources
- cache `images, css, js` files on CDN is a great improvement on performance on front end

![image](../assets/cdn_flow.png ':size=627x226')