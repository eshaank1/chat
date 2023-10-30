---
comments: false
layout: default
title: Chatroom
permalink: /chatroom
---
<html>
<head>
    <title>Chatroom</title>
    <link rel="stylesheet" href="/path/to/your/css/styles.css">
</head>
<body>
    <h1>Chatroom</h1>
    <div id="chat-box">
        <!-- Messages will be displayed here -->
    </div>
    <input type="text" id="message" placeholder="Type your message">
    <button id="send">Send</button>
    <!-- Script to send and receive messages -->
    <script>
        const chatBox = document.getElementById("chat-box");
        const messageInput = document.getElementById("message");
        const sendButton = document.getElementById("send");
        const backendUrl = "http://127.0.0.1:8987/api/chats/create";
        sendButton.addEventListener("click", sendMessage);
        function sendMessage() {
            const message = messageInput.value;
            if (message) {
                // Create a new message element
                const messageElement = document.createElement("div");
                messageElement.textContent = message;
                // Append the message to the chat box
                chatBox.appendChild(messageElement);
                // Send the message to the backend
                fetch(backendUrl, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({ message: message }),
                })
                    .then((response) => {
                        if (response.status === 200) {
                            messageInput.value = ""; // Clear the input field
                        }
                    })
                    .catch((error) => {
                        console.error("Failed to send message to the backend:", error);
                    });
            }
        }
        // Function to periodically retrieve and display chat messages
        function displayChat() {
            // Fetch chat messages from the backend
            fetch("http://127.0.0.1:8987/api/chats/read")
                .then((response) => response.json())
                .then((data) => {
                    // Clear the chat box
                    chatBox.innerHTML = "";
                    // Display each message in the chat box
                    data.forEach((message) => {
                        const messageElement = document.createElement("div");
                        messageElement.textContent = message.message;
                        chatBox.appendChild(messageElement);
                    });
                })
                .catch((error) => {
                    console.error("Failed to retrieve chat messages:", error);
                });
        }
        // Retrieve and display chat messages initially and every few seconds
        displayChat();
        setInterval(displayChat, 5000); // Update the chat every 5 seconds
    </script>
</body>
</html>
