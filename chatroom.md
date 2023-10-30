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
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
        #chat-container {
            width: 80%;
            max-width: 800px;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #333; /* Darker background color */
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        #chat-box {
            width: 100%;
            min-height: 400px;
            max-height: 400px; /* Set a maximum height */
            overflow-y: scroll; /* Add scrollbars if needed */
            border: 1px solid #ccc;
            border-radius: 10px;
            padding: 20px;
            background-color: #444; /* Even darker background color */
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        #message-container {
            display: flex;
            align-items: center;
        }
        #message {
            flex: 1;
            padding: 12px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
            background-color: #333; /* Darker background color */
            color: #fff; /* White text color */
        }
        #send {
            background-color: #007BFF;
            color: #fff;
            border: none;
            border-radius: 5px;
            padding: 12px 20px;
            cursor: pointer;
        }
    </style>
</head>

<body>
    <h1>Chatroom</h1>
    <div id="chat-container">
        <div id="chat-box">
            <!-- Messages will be displayed here -->
        </div>
        <div id="message-container">
            <input type="text" id="message" placeholder="Type your message" onkeypress="handleKeyPress(event)">
            <button id="send" onclick="sendMessage()">Send</button>
        </div>
    </div>
    <!-- Script to send and receive messages -->
    <script>
        const chatBox = document.getElementById("chat-box");
        const messageInput = document.getElementById("message");
        const backendUrl = "http://127.0.0.1:8987/api/chats/create";
        function sendMessage() {
            const message = messageInput.value.trim();
            if (message !== '') {
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
                            messageInput.value = ''; // Clear the input field
                        }
                    })
                    .catch((error) => {
                        console.error("Failed to send message to the backend:", error);
                    });
            }
        }
        function handleKeyPress(event) {
            if (event.key === "Enter") {
                event.preventDefault();
                sendMessage();
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
