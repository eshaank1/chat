---
comments: false
layout: default
title: Chatroom
permalink: /chatroom
---

<html>

<head>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #301934;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chatroom {
            width: 700px;
            height: 600px;
            background-color: #000;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        .chatroom-header {
            background-color: #301934;
            color: #000;
            text-align: center;
            padding: 10px;
            border-bottom: 1px solid #301934;
        }
        .chatroom-messages {
            max-height: 430px;
            min-height: 430px;
            padding: 8px;
            overflow-y: auto;
            background-color: #000;
            scrollbar-width: thin; /* for Firefox */
            scrollbar-color: #301934 #000; /* for Firefox */
        }
        .chatroom-messages::-webkit-scrollbar {
            width: 8px; /* for Chrome, Safari, and Opera */
        }
        .chatroom-messages::-webkit-scrollbar-thumb {
            background-color: #301934; /* for Chrome, Safari, and Opera */
        }
        .chatroom-messages div {
            background-color: #000;
            border-radius: 5px;
            margin: 5px 0;
            padding: 10px;
            word-wrap: break-word;
        }
        .chatroom-input {
            padding: 10px;
            display: flex;
            border-top: 1px solid #FFFFFF;
        }
        input[type="text"] {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 5px;
            background-color: #301934;
            color: #FFFFFF;
        }
        button {
            background-color: #301934;
            color: #FFFFFF;
            border: none;
            border-radius: 5px;
            padding: 10px 20px;
            cursor: pointer;
            margin-left: 10px;
        }
    </style>
</head>

<body>
    <div class="chatroom">
        <div class="chatroom-header">
            <h1>Chatroom</h1>
        </div>
        <div class="chatroom-messages">
            <!-- Messages will be displayed here -->
        </div>
        <div class="chatroom-input">
            <input type="text" id="message" placeholder="Type your message" onkeypress="handleKeyPress(event)">
            <button id="send" onclick="sendMessage()">Send</button>
        </div>
    </div>
    <!-- Script to send and receive messages -->
    <script>
        const chatBox = document.querySelector(".chatroom-messages");
        const messageInput = document.getElementById("message");
        const backendUrl = "https://chat.stu.nighthawkcodingsociety.com/api/chats/create";
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
            fetch("https://chat.stu.nighthawkcodingsociety.com/api/chats/read")
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
        setInterval(displayChat, 2000); // Update the chat every 5 seconds
    </script>
</body>

</html>
