---
comments: false
layout: default
title: Chatroom
permalink: /chatty
---

<html>
<head>
    <title>Chat Application</title>
</head>
<body>
    <h1>Chat Application</h1>
    <div id="chat-box">
        <!-- Messages will be displayed here -->
    </div>
    <input type="text" id="message" placeholder="Type your message">
    <button id="send">Send</button>
    <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
    <script>
        (document).ready(function() {
            // Function to retrieve and display chat messages
            function displayChat() {
                get("http://127.0.0.1:8987/api/chats/read", function(data) {
                    // Clear the chat box
                    ("#chat-box").empty();
                    // Append each message to the chat box
                    data.forEach(function(message) {
                        ("#chat-box").append("<p>" + message + "</p>");
                    });
                });
            }
            // Initial display of chat messages
            displayChat();
            // Function to send a new message to the backend
            function sendMessage() {
                var message = $("#message").val();
                if (message) {
                    post("http://127.0.0.1:8987/api/chats/create", JSON.stringify({ "message": message }), function(data) {
                        console.log(data);
                        // Clear the input field
                        ("#message").val("");
                        // Refresh the chat messages
                        displayChat();
                    });
                }
            }
            // Event listener for the Send button
            ("#send").click(sendMessage);
            // Event listener for Enter key in the message input field
            ("#message").keypress(function(e) {
                if (e.which === 13) { // 13 is the Enter key code
                    sendMessage();
                }
            });
        });
    </script>
</body>
</html>
