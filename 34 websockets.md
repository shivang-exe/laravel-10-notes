##                                          Web Sockets

If we talk about realtime data transfer in laravel we can do so using various methods :-

1. Polling ( uses Http requests )

Consider a realtime chat app, Sender sents a message to reciever, following will occur
first it updates to server.
second server updates the DB.

A single req. is coming from reciever in every 5 secs to check the updates, if updates are available on DB then reciever will got thier new message. 


`The process is quiet slow therefore it is not used.`
`req/res contains a lot of data with itself which is not needful in realtime chat`
`A large bandwidth is required if lot of comm. will occur between sender and reciever`

# We need something which is fast, lightweight and supported by the browser, here comes the savior-    WebSockets


Web socket is another protocol like http to communicate. Once the connection btw client and server is established it will remain persisted and maintained, while in http the connection will closed as soon as we get response from server.


# PUB - SUB pattern

Publisher-Subscriber Model ( One way communication )
Publisher Subscriber pattern is used widely in realtime financial ( stocks ) app, live streaming from camera. 


# RPC pattern

Remote procedure call ( Two way communication )

Step 1: Server establish a connection with client.

Step 2: Client sent a request to server along with some data.

Step 4: Server process it and send back reply to client.

Repeat 2 & 4  till connection is closed.

ex: A chat application.



# Services that provides WebSocket Server:

1. Pusher
2. Ably
3. Laravel WebSocket ( open source )
4. Laravel echo server ( open source --node )
5. solceti ( open source --node )



Going with laravel websocket
cmd- `composer require beyondcode/laravel-websockets`

After installing we need to publish files.
cmd- `php artisan vendor:publish`
choose - 2nd

Migrate files:
cmd- `php artisan migrate`


cmd- `composer require pusher/pusher-php-server`

# dont forget to uncomment the Broadcast Service provider in config/app.php
run - `php artisan websockets:serve`


Laravel broadcast messages in three types of channels:
1. public channel       - Does'nt Reqiures Authentication
Anyone in this world can join the channel

2. presence channel     - Reqiures Authentication
Each person details is open in the channel. 
Best for chat app where we can identify who is online and who is offline.

3. private channel      - Reqiures Authentication
Users does not know details of each other.



` Go to channel.php`
{{

Broadcast::channel('App.Models.User.{id}', function ($user, $id) { //user is instance of current logged-in user
    return (int) $user->id === (int) $id;
});

// For private channels we need to return a boolean if it is true then, user is authorised for this channel.
// For presence channels we need to return a user info for this channel.


}}



Theory: 

client make an http req to server on path : `/broadcasting/auth` after this server will execute a callback written in channels.php then server give response back to client with a meta-data containing connection details.
This is called `TLS handshake`










