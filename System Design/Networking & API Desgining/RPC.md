RPC (==Remote Procedure== call) is a technique that allow a program to call a another procedure (function or method) that is located in a different address space. 
## Key Characteristics of a Remote Procedure:

1. **Located on a Remote System:** A remote procedure resides on a different machine, address space, or server compared to the client that initiates its execution.
2. **Callable by Remote Clients:** Clients can invoke and execute this procedure from a remote location as if it were a local function or method call.
3. **Abstracted Communication:** The communication between the client and the remote procedure is abstracted, allowing the client to interact with the remote procedure without needing to handle the low-level networking details.
4. **Transparent Invocation:** The client calling the remote procedure typically uses programming language constructs that make the call appear similar to a local procedure call, even though it’s executed remotely

## Key components of RPC
1. ~={red}Client=~: The program or process that initiates the RPC by requesting a service from a remote server.
2. ~={red}Server=~: The program or process that provides the requested service. It listens for incoming RPC requests and executes the corresponding procedures.
3. ~={red} Stub =~: Also known as a **client proxy**, it is a local representation of the remote service. The client interacts with the stub as if it were the actual service, and the stub handles the communication details.
4. ~={red}Skeleton=~: Also known as a **server proxy**, it represents the server’s interface to the client. It receives incoming RPC requests, unpacks the parameters, and invokes the appropriate procedures on the server.
5. ~={red}Communication Protocol=~: Defines the rules and formats for exchanging messages between the client and server. Protocols like HTTP, TCP/IP, and gRPC are commonly used for RPC
## How RPC Works

When a client invokes a remote procedure, the following steps typically occur:

1. **Client Stub Invocation:** The client calls a procedure that appears local but is actually a stub representing the remote procedure. The stub prepares an RPC request, including information about the procedure to execute and its parameters.
2. **Marshalling:** The parameters and method information are serialized (converted into a format suitable for transmission) to be sent over the network.
3. **Communication:** The client sends the RPC request to the server using a chosen communication protocol.
4. **Server Skeleton Processing:** The server’s skeleton receives the request, unpacks the data (demarshalling), and determines the requested procedure.
5. **Local Procedure Execution:** The server executes the requested procedure using the provided parameters.
6. **Response Preparation:** The server marshals the response (if any) into a format for transmission.
7. **Response Transmission:** The server sends the response back to the client.
8. **Unmarshalling:** The client stub receives the response, demarshals it, and returns the result to the client as if it were a local procedure call.