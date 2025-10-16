# Bruh, we reviewing and shit

## Multisystems

### Sync or Async systems
- its important to look into this, specially how Corba would handle it.
- Message control is fundamental, we briefly saw it lol
- We followed the lab:
  - Pipes are useful :3, we wanted to study this to get Sockets
  - Protocols! They are important because systems are sending BYTES, and the receiver doesnt even know when the message has been finished
  - Thats why sockets usually pre determine the protocol, (little endian?)
  - sockets and those protocol suck, they are hard and comborsome to build, they usually mean more work.
  - Sockets are important because they are always present, if its in CORBA or WebService, they all use sockets.
  - so usually you would need to know the `ip` and the `port` to make a connection and start messaging.
  - ip and port (client and server, so that would be **4** crucial values for making connections)
  - lol be sure to look at the labs and roteiros for a better dive

### Parallel activities because of Multisystems
for every new connection, a thread is created which will handle RX and TX, and if those multiple threads fuck up, it will cause race condition. So we will need to use semaphores/mutex and stuff. Interprocess concurrency! This topic will not be heavily charged.

What is quite important for us is the Interprocessing concurrency. We need to make orchestrate the process, because sometimes a process needs to wait for another process to do something. 
 - That can be done with exchanging messages
 - BUt just messages, if it grows, it fucks

SO, to solve that:
 - Zookeeper
 - etcd

Both of these are meant to solve Interprocess concurrency. Take a look at the labs of those to better understand those.
etcd looks more like a databse, because you define a key and the definition of it, but its distributed! You can establish a `watch` with it. That can really orchestrate stuff.

CHECK TDE because its the subject of this.

## Middlewares
We basically saw 2 (the third will be for the next test) middlewares. Which are:
 - OOP (CORBA)
 - Services (uses web)

### Introduction
 - RPC (Remote Procedure Call): Basically, we isolate the implementation from the interface (I guess?). Basically, stubs and proxy! it will handle the hard stuff of connections
 - OOP: the methods are accesible via web, so you can call a method from another computer.
 - CORBA
   - IDL: Interface Descriptive Language. We describe the methods and variables
   - After compiling that: we get **STUBS** (client) and **SKELETON** (server)
   - Server runs and generates **IOR** which is basically a ticket that can access the client. Funny that even if the server dies, the IOR will dies as well if its TRANSIENT. Now if its PERSISTENT, you can still use it even if the server shutdown!

 - WebService:
   - We use the web infrastructure (its protocol) and make the web to send messages! Asking for services
   - We can do it in 2 ways: SOAP and REST:
     - SOAP: Its **XML** based. Full of tags and stuff, and the tags can be any field you want.
       - but you can also use it to describe interfaces, which is known for **WSDL** (equivalent to IDL, but for WebServices)
       - we used **zeep** in the labs to understand SOAP.
     - REST: its **resource** based. For every resource, we have a specific URL for it. (resource is anything that interests you lol)
       - Its either **GET**, **POST**, **PATCH**, **PUT** and **DELETE**. (they are from HTTP protocol)
         - GET: just gives me the resource (READ)
         - POST: creates another resource (that didnt exist before) (CREATE)
         - PATCH: a modification on the resource (UPDATE)
         - PUT: It basically modifies, but actually substitutes the old one with the new resource with the new modifications (UPDATE)
         - DELETE: deletes the resource (DELETE)

         - This system is called `CRUD` (Create, Read, Update and Delete)
         - during our lab, we used Flask-RESTFUL, and we defined our API. Basically saying what happens when we get a GET and so on.
         - each resource needs to have its own URL, this is important!! 


bruh, doing the labs would help (said the professor hmm)

The professor said that the test will be mixed with multiple choices and some open questions. nothing about the syntax, but there will be maybe questions about how you imeplemented the Projects, and conceptual questions of the discipline and of the big questions 
