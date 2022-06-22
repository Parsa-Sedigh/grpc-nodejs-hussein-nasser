Video: https://www.youtube.com/watch?v=Yw4rkaTc0f8&t=2229s&ab_channel=HusseinNasser

## agenda:
- motivation
  - client/server communication
  - problem with client libraries
  - why grpc was invented?
- grpc
  - unary grpc
  - server streaming
  - client streaming
  - bidirectional
- coding
- grpc pros & cons
- I'm tired of new protocols
- summary
---

This protocol uses http2 as an underlying transport mechanism and it uses protocol buffer as the message format.

Modes of grpc:
- Unary grpc is the traditional request/response system which is popular.
- server streaming: Like you're watching youtube and server is sending info but the client doesn't ask for it.
- client streaming: where the client sending a lot of information but the server doesn't respond like uploading a file. 
- bidirectional: Like chatting. 

## client-server communication
- soap, rest, graphql
- SSE, websockets
- raw TCP

SOAP is almost like RPC. You establish a communication between two parties(client and server). The client has to have a library that understands
SOAP(and XML?). The server has to have a library that understands SOAP and XML. The client and server has to agree on schema(how would the messages that 
I'm gonna send, looks like?).

People hate it because of bloated XML and the schema. That's why REST was built. In REST we don't care about schema.

The first 3 protocols are not bidirectional.

These things didn't solve certain problems which is: What if I need bidirectional communications, not just a request/response.

Most databases invent their own protocol on top of raw TCP. Because TCP is bidirectional binary. With it, we're gonna define our own protocol or message 
format.

## The problem with client libraries
No matter what you choose as a communication, you have your client that sends this communication and the server which receives the communication, HAVE TO AGREE.
How do they agree?
You need a client library. If you want to build a REST API, you need a client library. It is the http library and if you're lucky and if you're
building a web application, you just saved your time, you don't need a client library, because who's you client in this case? It's the browser.
The browsers are the biggest http client library on existence.

Having a client library is a huge deal. Because, yeah, if it's a browser, you don't really care. Because if you make a fetch req or xhr in browser
which means you called a browser method, the browser ACTUALLY establishes a http communication with the server, it negotiates 
the protocol via ALPN, it does the TLS for you, it makes sure the server actually supports http2, if it doesn't, the browser falls back
to http 1, it does the headers of req, it does the streams and ... , so you don't have to do anything. You just make your req and browser takes care of it.

But what if you're not on the browser?
In that case, you need a http client library. Or in case of SOAP, you need a soap library. You need a library that understands the protocol.

So the problems with http libraries:
Hard to maintain, if you want to add a new feature, you need to patch them.

- any communication protocol needs client library for the language of choice
  - SOAP library
  - HTTP client library
- hard to maintain and patch client libraries
  - http/1.1 , http/2 , new features, security, ...

Also in case of websockets, you need a library that understands how to talk with websockets. If you're using browser, problem solved, because chrome is 
maintaining the library.

and there's where GRPC came into...

## Why gRPC was invented?
