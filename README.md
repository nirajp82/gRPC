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
<p>As we’ve seen before, the data is transmitted as Protobuf based on a configuration known as messages. The messages are kept in .proto files. Let's look at a message example:</p>
    ```
    syntax = "proto3";
    message Person {
      uint64 id = 1;
      string email = 2;
      bool is_active = 3;
      enum PhoneType {
        MOBILE = 0;
        HOME = 1;
        WORK = 2;
      }
       message PhoneNumber {
        string number = 1;
        PhoneType type = 2;
       }

      repeated PhoneNumber phones = 4;
    }
    ```
  <p>From the above example, we can see a message is declared with a message keyword followed by the user-defined message name. The literals or the components are declared within the curly brackets. Each literal field can be divided into four components. They are:
</p>
<p></p>
    <p></p>
<p></p>
    <p></p>
<p></p>
    <p></p>
<p></p>
    <p></p>











References:
* https://grpc.io/
* https://www.youtube.com/watch?v=QyxCX2GYHxk&ab_channel=IAmTimCorey
* https://stackoverflow.com/questions/52146721/how-are-protocol-buffers-faster-than-xml-and-json
* https://en.wikipedia.org/wiki/Protocol_Buffers
* https://betterprogramming.pub/understanding-protocol-buffers-43c5bced0d47
