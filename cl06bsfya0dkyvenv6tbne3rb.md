## The entire process from entering a URL to page rendering

We've learned so many chapters before, and it's time to find a time to review and digest them again. Just borrow this classic interview question to connect the knowledge of the browser and network chapters that you have learned before.

The first is DNS query. If intelligent DNS resolution is done in this step, the IP address with the fastest access speed will be provided. This part of the content has not been written before, so I will explain it here.

# DNS

The role of DNS is to query the specific IP through the domain name.

Because IP has a combination of numbers and English (IPv6), it is not conducive to human memory, so domain names appear. You can think of a domain name as an alias of an IP, and DNS is to query what the real name of the alias is.

The DNS query is done before the TCP handshake, and this query is done by the operating system itself. When you want to visit www.google.com in your browser, you do the following:

1.  The operating system will first look up the IP in the local cache

2. If not, it will go to the DNS server configured in the system to query

3. If it has not been obtained at this time, it will directly go to the DNS root server to query. In this step, the query will find the server responsible for the first-level domain name of com.

4. Then go to the server to query the second-level domain name of google

5. The next third-level domain name query is actually configured by us. You can configure an IP for the www domain name, and then you can configure an IP for other third-level domain names.

The above is the DNS iterative query, and there is another kind of recursive query. The difference is that the former is made by the client to make the request, and the latter is made by the DNS server configured by the system, and the data is returned to the client after the result is obtained.

PS: DNS is a query based on UDP. You can also consider why you did not consider using TCP to implement it before.

Next is the TCP handshake, the application layer will send data to the transport layer, where the TCP protocol will specify the port numbers at both ends, and then send it to the network layer. The IP protocol in the network layer determines the IP address and instructs how to hop between routers in data transmission. Then the packet will be encapsulated into the data frame structure of the data link layer, and finally the transmission at the physical level.

In this part, the handshake situation of TCP and some characteristics of TCP can be described in detail.

When the TCP handshake is completed, the TLS handshake will be performed, and then the official data transmission will begin.

In this part, the TLS handshake and the content of the two encryption methods can be described in detail.

Before the data enters the server, it may also pass through the server responsible for load balancing. Its function is to distribute requests to multiple servers reasonably. At this time, it is assumed that the server will respond to an HTML file.

First, the browser will determine what the status code is. If it is 200, it will continue parsing. If it is 400 or 500, it will report an error. If it is 300, it will be redirected. There will be a redirect counter here to avoid excessive redirection. , an error will be reported if the number of times is exceeded.

The browser starts parsing the file. If it is in gzip format, it will decompress it first, and then know how to decode the file through the encoding format of the file.

After the file is decoded successfully, the rendering process will officially start. First, the DOM tree will be built according to the HTML, and if there is CSS, the CSSOM tree will be built. If a script tag is encountered, it will determine whether there is async or defer , the former will download and execute JS in parallel, the latter will download the file first, and then wait for the HTML parsing to complete and execute sequentially.

If none of the above, the rendering process will be blocked until the JS execution is complete. If you encounter a file download, you will download the file. If you use the HTTP/2 protocol here, it will greatly improve the download efficiency of multiple images.

After the CSSOM tree and the DOM tree are constructed, the Render tree will start to be generated. This step is to determine the layout, style and many other aspects of the page elements.

In the process of generating the Render tree, the browser starts to call the GPU to draw, synthesize the layer, and display the content on the screen.

This part is the content explained in the rendering principle, and this process can be explained in detail. And when downloading files, it can also be said that the problem of head-of-line blocking can be solved through the HTTP/2 protocol.

In general, this chapter is to take everyone from the DNS query to the rendering of the screen to fully understand the process, and connect the previously learned content.