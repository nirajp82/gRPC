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
 <p> The primary purpose of Protocol Buffers is to facilitate network communication, its simplicity and speed make Protocol Buffers an alternative to data-centric classes and structs, especially where interoperability with other languages or systems might be needed in the future. </p>
 <p> The definition of the data to be serialized is written in configuration files called proto files (.proto). These files will contain the configurations known as messages. The proto files can be compiled to generate the code in the user's programming language. <p>
  
#### Key Features of Protocol Buffers
  * <h5> Binary transfer format:</h5>
    <p> The Protobuf is a binary transfer format, meaning the data is transmitted as a binary. This improves the speed of transmission more than the raw string because it takes less space and bandwidth. Since the data is compressed, the CPU usage will also be less. </p>
    <p> The only disadvantage is the Protobuf files or data isn’t as human-readable as JSON or XML </p>
  * <h5> Separation of context And data:</h5>
    <p>In JSON and XML, the data and context aren’t separate — whereas in Protobuf, it is separate. Consider a JSON example.</p>
    ```
    {
    first_name: "Arun",
    last_name: "Kurian"
    }
    ```
    <p>In this example, the transmitted data has got an object literal with two properties, first_name and last_name, with values Arun and Kurian. This is highly readable, but this can take up more space. Here, every JSON message has to provide both of these pieces every single time. As our data grows, the transmission time will be increased significantly.</p>
    <p>But in the case of Protobufs, things are different. We first define a message in a configuration file like this:</p>
    ```
      {
        string first_name = 1;
        string last_name = 2;  
      }
    ```
    <p>This configuration file contains the context information. The numbers are just identifiers of the fields. Don't worry if the message format is a bit confusing — we’ll look into it in detail later. By using this configuration, we can send encoded data as: </p>
    ```
    124Arun226Kurian
    ```
    <p>In the case of 124Arun, 1 stands for the field identifier, 2 for the data type (which is the string), and 4 is the length of the text. I admit this is a bit more difficult to read than JSON; however, this will take very little space compared to JSON data.</p>

<h5> Message Format:</h5>

<p>As we’ve seen before, the data is transmitted as Protobuf based on a configuration known as messages. The messages are kept in .proto files. Let's look at a message example</p>

<p> From the above example, we can see a message is declared with a message keyword followed by the user-defined message name. The literals or the components are declared within the curly brackets. Each literal field can be divided into four components. They are:</p>

<h6>Field rule:</h6>
<p>In the proto2 version of Protobuf, there were rules like required, optional, and repeated that were to be added before the field type or data type. This was optimized in proto3 and only the repeated rule is kept. A field is said to be repeated if the field represents an array of elements of the same type. If the field isn’t repeated, no rules should be added.</p>

<h6>Field types:</h6>
<p>The data types a field can hold fall into three categories.
The first is the scalar data types, like strings and numbers. The second is an enum data type. In our example, this is PhoneType. And the final data type is an embedded message (like the PhoneNumber in our example).
Like in JSON and XML, when using message types, we can build hierarchies of messages to represent data of any kind. The scalar data types available in Protobuf are float, int32, int64, uint32, uint64, sint32, sint64, fixed32, fixed64, sfixed32, sfixed6, bool, string, and bytes.</p>

<h6>Field names:</h6>
<p>When naming the fields in Protobuf, there are some conventions to be followed because these are assumed by the Protoc compiler as it generates code based on the .proto file for the language of your selection. The first convention is field names should all be in lowercase. Secondly, if there are multiple words in the field name, they should be separated by an underscore.</p>

<h6>Field tags:</h6>
<p>The field tags are a numeric representation of the field, and this enables us to have richly defined field names in the definition without sending them through the wire.
There are certain things to be considered when working with field tags. Firstly, the field tags must be unique inside a message. Secondly, they have to be integers. Thirdly, if a field is to be removed from the definition that’s already in use, its tag must be declared as reserved to prevent it from being redefined. Example: reserved 8; </p>

## Core concepts, architecture and lifecycle
### Service definition
<p>Like many RPC systems, gRPC is based around the idea of defining a service, specifying the methods that can be called remotely with their parameters and return types. By default, gRPC uses protocol buffers as the Interface Definition Language (IDL) for describing both the service interface and the structure of the payload messages. It is possible to use other alternatives if desired.</p>
<p>gRPC lets you define four kinds of service method:</p>
<h5>Unary RPCs:</h5>
<p>where the client sends a single request to the server and gets a single response back, just like a normal function call.</p>
```
rpc SayHello(HelloRequest) returns (HelloResponse);
```
<h5>Server streaming RPCs</h5>
<p>where the client sends a request to the server and gets a stream to read a sequence of messages back. The client reads from the returned stream until there are no more messages. gRPC guarantees message ordering within an individual RPC call.</p>
```
rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse);
```
<h5>Client streaming RPCs:</h5>
<p>Where the client writes a sequence of messages and sends them to the server, again using a provided stream. Once the client has finished writing the messages, it waits for the server to read them and return its response. Again gRPC guarantees message ordering within an individual RPC call.</p>
```
rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse);
```
<h5>Bidirectional streaming RPCs:</h5>
<p>where both sides send a sequence of messages using a read-write stream. The two streams operate independently, so clients and servers can read and write in whatever order they like: for example, the server could wait to receive all the client messages before writing its responses, or it could alternately read a message then write a message, or some other combination of reads and writes. The order of messages in each stream is preserved.</p>
```
rpc BidiHello(stream HelloRequest) returns (stream HelloResponse);
```

### Using the API 
<p>Starting from a service definition in a .proto file, gRPC provides protocol buffer compiler plugins that generate client- and server-side code. gRPC users typically call these APIs on the client side and implement the corresponding API on the server side.</p>
<p>On the server side, the server implements the methods declared by the service and runs a gRPC server to handle client calls. The gRPC infrastructure decodes incoming requests, executes service methods, and encodes service responses.</p>
<p>On the client side, the client has a local object known as stub (for some languages, the preferred term is client) that implements the same methods as the service. The client can then just call those methods on the local object, wrapping the parameters for the call in the appropriate protocol buffer message type - gRPC looks after sending the request(s) to the server and returning the server’s protocol buffer response(s).</p>

#### Synchronous vs. asynchronous
<p>Synchronous RPC calls that block until a response arrives from the server are the closest approximation to the abstraction of a procedure call that RPC aspires to. On the other hand, networks are inherently asynchronous and in many scenarios it’s useful to be able to start RPCs without blocking the current thread.</p>
<p>The gRPC programming API in most languages comes in both synchronous and asynchronous flavors. You can find out more in each language’s tutorial and reference documentation.</p>


### RPC life cycle
  * <h5> Unary RPC: </h5> First consider the simplest type of RPC where the client sends a single request and gets back a single response.
    * Once the client calls a stub method, the server is notified that the RPC has been invoked with the client’s metadata for this call, the method name, and the specified deadline if applicable.
    * The server can then either send back its own initial metadata (which must be sent before any response) straight away, or wait for the client’s request message. Which happens first, is application-specific.\
    * Once the server has the client’s request message, it does whatever work is necessary to create and populate a response. The response is then returned (if successful) to the client together with status details (status code and optional status message) and optional trailing metadata.\
    * If the response status is OK, then the client gets the response, which completes the call on the client side.\
  * <h5>Server streaming RPC: </h5>
  A server-streaming RPC is similar to a unary RPC, except that the server returns a <b> stream of messages in response </b> to a client’s request. After sending all its messages, the server’s status details (status code and optional status message) and optional trailing metadata are sent to the client. This completes processing on the server side. The client completes once it has all the server’s messages.
 * <h5> Client streaming RPC: </h5> 
A client-streaming RPC is similar to a unary RPC, except that the client sends a stream of messages to the server instead of a single message. The server responds with a single message (along with its status details and optional trailing metadata), typically but not necessarily after it has received all the client’s messages.
 * <h5> Bidirectional streaming RPC: </h5>
  <p>In a bidirectional streaming RPC, the call is initiated by the client invoking the method and the server receiving the client metadata, method name, and deadline. The server can choose to send back its initial metadata or wait for the client to start streaming messages.</p>
<p>Client- and server-side stream processing is application specific. Since the two streams are independent, the client and server can read and write messages in any order. For example, a server can wait until it has received all of a client’s messages before writing its messages, or the server and client can play “ping-pong” – the server gets a request, then sends back a response, then the client sends another request based on the response, and so on
</p>

#### Deadlines/Timeouts:
gRPC allows clients to specify how long they are willing to wait for an RPC to complete before the RPC is terminated with a DEADLINE_EXCEEDED error. On the server side, the server can query to see if a particular RPC has timed out, or how much time is left to complete the RPC.

Specifying a deadline or timeout is language specific: some language APIs work in terms of timeouts (durations of time), and some language APIs work in terms of a deadline (a fixed point in time) and may or may not have a default deadline.

#### RPC termination: 
In gRPC, both the client and server make independent and local determinations of the success of the call, and their conclusions may not match. This means that, for example, you could have an RPC that finishes successfully on the server side (“I have sent all my responses!") but fails on the client side (“The responses arrived after my deadline!"). It’s also possible for a server to decide to complete before a client has sent all its requests.

#### Cancelling an RPC:
Either the client or the server can cancel an RPC at any time. A cancellation terminates the RPC immediately so that no further work is done.

#### Warning: 
Changes made before a cancellation are not rolled back.

#### Metadata:
Metadata is information about a particular RPC call (such as authentication details) in the form of a list of key-value pairs, where the keys are strings and the values are typically strings, but can be binary data. Metadata is opaque to gRPC itself - it lets the client provide information associated with the call to the server and vice versa.\
Access to metadata is language dependent.

#### Channels: 
A gRPC channel provides a connection to a gRPC server on a specified host and port. It is used when creating a client stub. Clients can specify channel arguments to modify gRPC’s default behavior, such as switching message compression on or off. A channel has state, including connected and idle.\
How gRPC deals with closing a channel is language dependent. Some languages also permit querying channel state.




References:
* https://grpc.io/
* https://www.youtube.com/watch?v=QyxCX2GYHxk&ab_channel=IAmTimCorey
* https://stackoverflow.com/questions/52146721/how-are-protocol-buffers-faster-than-xml-and-json
* https://en.wikipedia.org/wiki/Protocol_Buffers
* https://betterprogramming.pub/understanding-protocol-buffers-43c5bced0d47
* https://grpc.io/docs/what-is-grpc/core-concepts/
