# persistent-messaging

You will implement an asynchronous message service consisting of a server process and three clients. Each client
process will connect to the server over a socket. The server should be able to handle all three clients concurrently.
Clients will prompt the user for a username. When a client connects to the server, its username should be displayed by
the server in real time. Two or more clients may not use the same username simultaneously. Should the server detect a
concurrent conflict in username, the client’s connection should be rejected, and the client’s user should be prompted to
input a different name.
The server will keep a cumulative log of previously used usernames and display those names on its GUI. The server
should indicate which of those usernames represent clients presently connected to the server and which are not
connected. Clients may reuse usernames and a client reusing an extant username should not be treated as a duplicate
in the log.
When the server encounters a new username, a unique message queue should be created for that username. This
queue may consist of any type of data structure, but an independent queue should exist for each name. This queue
must be able to contain an arbitrary number of messages.
When a message is received by the server, the server should place the message in the corresponding queue for that
intended recipient and mark that message with the time of its reception. Queues should be persistent (e.g., the contents should survive the server process restarting) even if there are no messages in that queue when the server
process is killed.
When a client connects to the server, the user should be prompted to select one of two options:
• Send message; or,
• Check for messages.
If the user selects “Send message”, the server should provide the log of usernames to the client. The user will then be
prompted to select from one of three options:
1. Send message to specific username (1-to-1)
2. Send message to a subset of usernames (1-to-n)
3. Send a message to all usernames (1-to-all)
If a user selects options 1 or 2, it is the developer’s discretion with regards to how the user indicates their selection. If
the user indicates their selection by typing a string, the client should reject that string if it does not match an entry in the
list received from the server.
Once the user has selected the intended recipient(s), the user should be prompted to enter a brief text message. When
the user has finished, the text message should be uploaded to the server.
If the user selects “Check for messages”, the server should deliver the contents of the message queue that corresponds
to that client’s present username. If the queue is currently empty, the user should be notified that no messages are
available. Once messages are retrieved by the client, the server should delete them from the queue.
The client should remain connected to the server until manually disconnected by the user.
The required actions are summarized below. 

Client
Startup:
1. Prompt the user to input a username.
2. Connect to the server over a socket and register the username.
a. When the client is connected, the user should be notified of the active connection.
b. If the provided username is already in use, the client should disconnect and prompt the user to input
another username.
3. Proceed to send and check for messages until manually killed by the user.
Sending Messages:
1. The client will present the list of usernames received by the server to the user.
2. The user will be prompted to select from one of the three messaging options listed above.
3. The user will be prompted to select their intended recipient(s).
4. After the recipients are selected, the user should be prompted to input a brief text message.
5. The client will upload the text message to the server.
6. Return to Sending Messages: Step 1 until manually disconnected by the user.
Checking Messages:
1. When connected to the server, the client will indicate it wants to retrieve messages from its message queue.

2. If the clients message queue is not empty:
a. The client will retrieve all text messages addressed to it; and,
b. Print the content of those text messages to its GUI.
c. The text message should indicate from which username the message was received and a timestamp of
when that message was received by the server.
3. Return to Receiving Messages: Step 1 until manually disconnected by the user.


Server

The server should support three concurrently connected clients. A cumulative log of all previously used usernames
should be maintained by the server and presented on the server’s GUI. The server should indicate which of those
usernames (if any) represent currently connected clients. The server will execute the following sequence of steps:
1. Startup and listen for incoming connections.
2. Print that a client has connected, log the client’s username, and:
a. If the client username is available (e.g., not currently being used by another client), fork a thread to
handle that client. Or,
b. If the username is in use, reject the connection from that client.
3. If a client name has not been encountered in the past, a new message queue for that client should be
instantiated.
4. The server will proceed according to whether the client wants to send or check for messages.
a. If a client indicates it wants to send a message:
i. The server should provide the log of usernames to the client;
ii. The server should accept messages from the client and place those messages in the intended
client(s) message queue;
iii. Received messages should be marked with a timestamp.
b. If a client indicates it wants to check for messages:
i. The server should send any available messages to the client; or,
ii. Indicate that no messages are available.
iii. Any messages delivered to the client should be removed from the persistent message queue.
5. Begin at step 2 until the process is killed by the user.

