## Web Network Protocol Comparison

#### I. [network performance: http1.0  vs http2](#question-1)

#### II. [network choices for Loading new Feeds](#question-2)
- [Long Polling](#q2-1)
- [Web Socket](#q2-2)
- [SSE](#q2-3)

#### III. [How CDN helps performance?](#question-3)

#### IV. [What is Message Queue](#question-4)

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

<div id="q2-1" />

#### 2.1 Long polling
This is a variation of the traditional polling technique that allows the server to push information to a client whenever the data is available.
- 1 ) The client makes an initial request using regular HTTP and then waits for a response.
- 2 ) The server delays its response until an update is available or a timeout has occurred.
- 3 )  When an update is available, the server sends a full response to the client.
- 4 ) The client typically sends a new long-poll request, either immediately upon receiving a response or after a pause to allow an acceptable latency period.
- 5 ) Each Long-Poll request has a timeout. The client has to reconnect periodically after the connection is closed due to timeouts.

**Advantages:**
- With long polling, the client may be configured to allow for a longer timeout period (via a Keep-Alive header) when listening for a response.
- a longer persisted connection on server side
- wait data response or **timeout**([304 Not Modified](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/304))

**Flow chart:**

![image](../assets/longpolling.png ':size=517x378')

<div id="q2-2" />

#### 2.2 Websocket
- 1 ) provides [Full duplex](https://en.wikipedia.org/wiki/Duplex_(telecommunications)#Full_duplex) communication channels over a single TCP connection. 
- 2 ) It provides a persistent connection between a client and a server that both parties can use to start sending data at any time.

<div id="q2-3" />

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


### IV. What is the Reverse Proxy?

What is proxy?
- Selectively Block / Send request between client & server
- log / monitor requests
- cache responses

1 ) Reverse Proxy: from client to server
	- Ngnix
	- Apache
	- ...
2 ) Forward Proxy: from server to client

<div id="question-4"/>

### IV. What is Message Queue?

#### 4.1 Definition

**Asynchronous** **service-to-service communication** used in serverless and microservices architectures.
 
A message queue provides an **asynchronous communications protocol,** which is a system that puts a message onto a message queue and does not require an immediate response to continuing processing.
- Producer - server node
- Consumer - server node

#### 4.2 How MQ works?
Rules:
- Each message is processed only once, by a single consumer
- Queues also have **fault tolerance,** they can **retry** the requests.

![image](../assets/mq_flow.png ':size=517x240')

Use case:
- email
- food order	
	![image](../assets/foodorder.png ':size=505x256')

#### 4.3 Advantages
- Decouple
- Scaling

A decoupled system is achieved when two or more systems are able to communicate without being connected. The systems can remain completely autonomous and unaware of other functions.

#### 4.4 Examples of MQ
- Apache Kafka
- RabbitMQ
- .....



