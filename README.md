# gRPC
### What is gRPC:
gRPC is a modern open source high performance Remote Procedure Call (RPC) framework that can run in any environment. 
1. It is a method of web communication between services. 
2. It relies on known configuration that is shared between the client and server, think of these like a contract. These contracts called Protocol buffers.
3. gRPC communicates using Binary Stream instead of JSON or XML.
  #### Dis-advantage of using text representation of data over Binary Stream
    * require text encode/decode (which can be cheap, but is still an extra step)
    * requires complex parse code, especially if there are human-friendly rules like "must allow whitespace"
    * usually involves more bandwidth - so more actual payload to churn - due to embedding of things like names, and (again) having to deal with human-friendly representations (how to tokenize the syntax, for example)
  
### What is Protocol Buffers
Protocol Buffers (Protobuf) is a open source cross-platform library used to serialize structured data. It is useful in developing programs to communicate with each other over a network or for storing data. The method involves an interface description language that describes the structure of some data and a program that generates source code from that description for generating or parsing a stream of bytes that represents the structured data. Think XML, but protobuf is smaller, faster, and simpler.\
__The primary purpose of Protocol Buffers is to facilitate network communication, its simplicity and speed make Protocol Buffers an alternative to data-centric classes and structs, especially where interoperability with other languages or systems might be needed in the future.\
__The definition of the data to be serialized is written in configuration files called proto files (.proto). These files will contain the configurations known as messages. The proto files can be compiled to generate the code in the user's programming language.
#### Key Features of Protocol Buffers
  * *Binary transfer format*:\
    The Protobuf is a binary transfer format, meaning the data is transmitted as a binary. This improves the speed of transmission more than the raw string because it takes less space and bandwidth. Since the data is compressed, the CPU usage will also be less.
The only disadvantage is the Protobuf files or data isn’t as human-readable as JSON or XML









References:
* https://grpc.io/
* https://www.youtube.com/watch?v=QyxCX2GYHxk&ab_channel=IAmTimCorey
* https://stackoverflow.com/questions/52146721/how-are-protocol-buffers-faster-than-xml-and-json
* https://en.wikipedia.org/wiki/Protocol_Buffers
* https://betterprogramming.pub/understanding-protocol-buffers-43c5bced0d47
