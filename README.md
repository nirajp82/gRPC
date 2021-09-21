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
  *





References:
* https://grpc.io/
* https://www.youtube.com/watch?v=QyxCX2GYHxk&ab_channel=IAmTimCorey
* https://stackoverflow.com/questions/52146721/how-are-protocol-buffers-faster-than-xml-and-json
