## what you do not know TLS

HTTPS still transmits information through HTTP, but the information is encrypted through the TLS protocol.

The TLS protocol is above the transport layer and below the application layer. The first TLS protocol transmission requires two RTTs, which can then be reduced to one RTT through Session Resumption.

There are two encryption techniques used in TLS: symmetric encryption and asymmetric encryption.

# Symmetric encryption:

Symmetric encryption means that both sides have the same secret key, and both sides know how to encrypt and decrypt the ciphertext.

This encryption method is good, but the problem is how to let both parties know the secret key. Because the transmitted data is all over the network, if the secret key is transmitted through the network, once the secret key is intercepted, there is no meaning of encryption.

# Asymmetric encryption:

There is a public key and a private key. Everyone can know the public key, and the data can be encrypted with the public key, but the private key must be used to decrypt the data. The private key can only be known by the party that distributed the public key.

This encryption method can perfectly solve the problem of symmetric encryption. Assuming that both ends need to use symmetric encryption now, before that, asymmetric encryption can be used to exchange keys.

The simple process is as follows: First, the server publishes the public key, and then the client also knows the public key. Next, the client creates a secret key, then encrypts it with the public key and sends it to the server. After the server receives the ciphertext, it decrypts the correct secret key with the private key. At this time, both ends know what the secret key is.

The TLS handshake process is as follows:

![1635260126b3a10c.webp](https://cdn.hashnode.com/res/hashnode/image/upload/v1645669954458/2sf81bTG8.webp)

The client sends a random value along with the desired protocol and encryption.

The server receives the random value of the client, and generates a random value by itself, and uses the corresponding method according to the protocol and encryption method required by the client, and sends its own certificate (if you need to verify the client certificate, you need to explain)

The client receives the certificate of the server and verifies whether it is valid. After the verification, a random value will be generated, and the random value will be encrypted by the public key of the server certificate and sent to the server. If the server needs to verify the client certificate, it will be attached Certificate

The server receives the encrypted random value and decrypts it with the private key to obtain the third random value. At this time, both ends have three random values. The key can be generated according to the previously agreed encryption method through these three random values. The next communication can be encrypted and decrypted by this key

Through the above steps, it can be seen that in the TLS handshake stage, both ends use asymmetric encryption to communicate, but because the performance of asymmetric encryption loss is greater than that of symmetric encryption, when formally transmitting data, both ends use symmetric encryption to communicate.

PS: The above descriptions are all about the handshake of the TLS 1.2 protocol. In the 1.3 protocol, only one RTT is required to establish a connection for the first time, and no RTT is required to restore the connection later.