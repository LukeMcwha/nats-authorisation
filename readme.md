# What is NATs

## What are the different things you can do with NATs?

### Publish
NATs lets you send messages to a subject. Like a HTTP request it takes a body and headers.

### Subscribe
NATs also lets you subscribe to subjects. It gets the body, headers from a publish request and can process the message and send a reply a reply subject specified within the message.

### Reply Subjects
Every message comes with a reply subject; it creates a short lived subscription to a randomly generated inbox. This allows a subscription to know where to send a response and creates the publish / subscribe pattern.


# Demonstration one - send to anything

Create a subscriber -> nats sub "*" --queue="rpc"
Publish a message -> nats pub rpc/hello/there 'hello there'

Catch all will see everything that comes in and tell you the endpoint it is coming from.

### Using / in the subject name stops you from doing many things
Sub - nats sub "rpc/>" --queue="rpc" -> this should let you see any messages that start with rpc/ and end with anything else
Pub nats pub rpc/hello/there 'hello there' -> see it doesn't work


### Changing them to dots allows you to do this.

Sub - nats sub "rpc.>" --queue="rpc"
Pub - nats pub rpc.hello.there 'hello there' -> receives message


### Tools NATs provides to accomplish this
#### Authentication Types
NATs has a few ways to authenticate a connection to the server. I'm going to focus on one of the easier ones of username / password but other options include: token, NKey, File Credentials and TLS.
When setting your username and password you connect to the NATs cluster; it will check its user configuration to understand what the connection has permission to.

#### Authorisation



### Lets give to a problem we are solving

We have 3 main services apart of Twist
- RGS - This is our key API all traffic goes through here.
- Game Engines - these are all the same except with different names. They give a bet result from the game to give to the RGS.
- RNG server - This generate our random numbers for the game.

We have these rules that we must achieve
- RGS can send requests to Game.
-- subject
- RGS can receive replies from the Game Engine.
- RGS can reply to a Game.
- Game can send replies to the RGS
- Game can send request to the RGS
- Game can send request to the RNG
- Game can receive replies from RNG



## Little inbox issues to also be aware of
- _INBOX.> allows you to connect to every inbox and listen to subscribe to other inboxes RESPONSE messages
- _INBOX.{servicename}.> - Making your subscribe subject for inboxes helps protect against this.
- _INBOX.> is needed for publish permissions unless you map all the other services that will send messages to this users subscription. A lot of effort.