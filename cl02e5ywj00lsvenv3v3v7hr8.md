## HTTP/2 and HTTP/3

In this chapter we will learn about HTTP/2 and HTTP/3 in the future.

HTTP/2 has solved some performance problems of the most commonly used HTTP/1. Just upgrading to this protocol can reduce a lot of performance optimization work that needs to be done before. Of course, compatibility issues and how to gracefully downgrade should be domestic One of the reasons it is not widely used.

Although HTTP/2 has solved many problems, it does not mean that it is perfect. HTTP/3 was introduced to solve some of the problems existing in HTTP/2.

# HTTP/2

Compared with HTTP/1, HTTP/2 can be said to greatly improve the performance of web pages.

In HTTP/1, for performance considerations, we will introduce sprite images, inline small images, use multiple domain names, and so on. All of this is because the browser limits the number of requests under the same domain name (under Chrome is generally limited to six connections), when a page needs to request a lot of resources, the head of line blocking will lead to When the maximum number of requests is reached, the remaining resources need to wait for other resource requests to complete before initiating requests.

Multiplexing technology was introduced in HTTP/2, which can transmit all request data through only one TCP connection. Multiplexing is a good solution to the problem that browsers limit the number of requests under the same domain name, and it is also easier to achieve full-speed transmission. After all, a new TCP connection needs to slowly increase the transmission speed.

You can feel how much faster HTTP/2 is than HTTP/1 through this link.

![163542ca61eaff17.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1645791041713/wAm989SIc.webp)

In HTTP/1, because of the blocking of the head of the queue, you will find that the sending request is long like this

![163542c96df8563d.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1645791091467/pHqr5u7kn.webp)

In HTTP/2, because you can reuse the same TCP connection, you will find that the sending request is long like this

![163542c9d3128c7a.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1645791113567/E72n0-v77.webp)

# binary transfer

This is the core of all the performance enhancements in HTTP/2. In previous versions of HTTP, we transmitted data by text. A new encoding mechanism was introduced in HTTP/2, all transmitted data is split and encoded in binary format.

![163543c25e5e9f23.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1645791216609/oOXDKR-ED.webp)

# multiplexing

In HTTP/2, there are two very important concepts, frame and stream.

A frame represents the smallest unit of data, and each frame identifies which stream the frame belongs to. A stream is a data stream composed of multiple frames.

Multiplexing means that there can be multiple streams in a TCP connection. In other words, multiple requests can be sent, and the peer can know which request belongs to through the identifier in the frame. Through this technology, the head-of-line blocking problem in the old version of HTTP can be avoided, and the transmission performance can be greatly improved.

![1635442531d3e5ee.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1645791356161/w511_XwBG.webp)

# Header compression

In HTTP/1, we use text to transmit headers, and in the case of headers carrying cookies, it may be necessary to repeatedly transmit hundreds to thousands of bytes each time.

In HTTP/2, the transmitted header is encoded using the HPACK compression format, reducing the size of the header. Index tables are maintained at both ends to record the headers that have appeared. Later, during the transmission process, the key names of the recorded headers can be transmitted. After the peer receives the data, the corresponding value can be found by the key name.

# PushServer Push

In HTTP/2, the server can actively push other resources after a client request.

It is conceivable that the client will request some resources. At this time, the server-side push technology can be adopted to push the necessary resources to the client in advance, so that the delay time can be relatively reduced. Of course you can also use prefetch if the browser is compatible.

# HTTP/3

Although HTTP/2 solves many problems of previous versions, it still has a huge problem, although this problem is not caused by itself, but the problem of the underlying TCP protocol.

Because HTTP/2 uses multiplexing, generally only one TCP connection needs to be used under the same domain name. When there is packet loss in this connection, it will cause HTTP/2 to perform worse than HTTP/1.

Because in the case of packet loss, the entire TCP has to start waiting for retransmission, which causes all subsequent data to be blocked. However, for HTTP/1, multiple TCP connections can be opened. In this case, only one of the connections will be affected, and the remaining TCP connections can still transmit data normally.

Then someone may consider modifying the TCP protocol, which is actually an impossible task. Because TCP has existed for too long, it has been flooded in various devices, and this protocol is implemented by the operating system, and it is not realistic to update it.

For this reason, Google started a QUIC protocol based on the UDP protocol and used it on HTTP/3. Of course, HTTP/3 was previously called HTTP-over-QUIC. From this name, we can also find that, The biggest transformation of HTTP/3 is the use of QUIC, and then we will learn about this protocol.

# QUIC

We have learned the content of the UDP protocol before and know that although this protocol is very efficient, it is not so reliable. Although QUIC is based on UDP, it has added many functions on the original basis, such as multiplexing, 0-RTT, encryption using TLS1.3, flow control, orderly delivery, retransmission and other functions. Here we select several important functions to learn the content of this protocol.

## multiplexing

Although HTTP/2 supports multiplexing, the TCP protocol does not have this function after all. QUIC implements this function natively, and a single data stream transmitted can be guaranteed to be delivered in an orderly manner without affecting other data streams. This technology solves the previous problems of TCP.

And QUIC will perform better on mobile than TCP. Because TCP is based on IP and port to identify connections, this method is very fragile in the changing mobile network environment. But QUIC identifies a connection by ID. No matter how your network environment changes, as long as the ID remains the same, you can quickly reconnect.

## 0-RTT

By using a technology similar to TCP fast open, the context of the current session is cached, and when the session is resumed next time, the previous cache only needs to be passed to the server for verification and transmission.

## Error correction mechanism

Suppose I want to send three packets this time, then the protocol will calculate the XOR value of these three packets and send a separate check packet, that is, a total of four packets are sent.

When the non-checking packet is lost, the content of the lost data packet can be calculated from the other three packets.

Of course, this technique can only be used when one packet is lost. If multiple packets are lost, the error correction mechanism cannot be used, and only retransmission can be used.

# summary

- HTTP/2 has greatly improved performance through multiplexing, binary streaming, header compression and other technologies, but there are still problems

- QUIC is implemented based on UDP and is the underlying support protocol in HTTP/3. The protocol is based on UDP and takes the essence of TCP to realize a fast and reliable protocol.
