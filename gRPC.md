---
id: gRPC
aliases:
  - gRPC
tags: []
---

# gRPC

## Overview
Basically it is calling a function but not from the current program, instead the function is from a different remote server which then process the request and respond with the result.
RPC in general is used for following reasons
- scale
- security
- easy upgrades

2 parts to RPC
1. Medium of transport
    HTTP2 used by gRPC
2. Serialization Format
    protobuf used by gRPC

gRPC uses HTTP/2 which provides several benifits over HTTP/1
- header compression - smaller requests
- push - send data without the client requesting it
- multiplexing requests over single TCP connection - save bandwidth??

protobuf is a binary serialization format
we define messages using certain data types

Client will serialize request, send to server, server deserialize, process the RPC request, serialize response and send back to client which will deserialize and use the result data

## Protocol buffers

Each field has an id. If we end up changing a field from our response struct, we add a new field with an incremented id.
This way the old ones stay backward compatible.

## gRPC server
## gRPC client

---

gRPC can be implemented in various ways,
1. unary - standard req res method
2. server streaming
3. client streaming

> Streams run over HTTP2 chunk transfer encoding but gRPC makes it easier.


