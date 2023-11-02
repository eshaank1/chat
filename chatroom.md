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
            max-height: 460px;
            min-height: 460px;
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
    const backendUrl = "https://chat.stu.nighthawkcodingsociety.com/api/chats"; // Base URL for chat API
    // Function to send a message to the server
    function sendMessage() {
        const message = messageInput.value.trim();
        if (message !== '') {
            const timestamp = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
            const messageWithTimestamp = `[${timestamp}] ${message}`;
            // Create a new message element
            const messageElement = document.createElement("div");
            messageElement.textContent = messageWithTimestamp;
            // Append the message to the chat box
            chatBox.appendChild(messageElement);
            // Ensure only the last 50 messages are displayed
            const messages = chatBox.querySelectorAll("div");
            if (messages.length > 50) {
                chatBox.removeChild(messages[0]);
            }
            document.getElementById("message").value = "";
            // Send the message to the server using the /create endpoint
            fetch(backendUrl + '/create', {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify({ message: messageWithTimestamp }), // Send the message with timestamp
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
    // Function to scroll to the bottom of the chatbox
    function scrollToBottom() {
        chatBox.scrollTop = chatBox.scrollHeight;
    }
    // Function to periodically retrieve and display chat messages
  let shouldScrollToBottom = true; // Flag to control auto-scrolling
// Function to scroll to the bottom of the chatbox if already at the bottom
function scrollToBottomIfAtBottom() {
    if (shouldScrollToBottom) {
        chatBox.scrollTop = chatBox.scrollHeight;
    }
}
// Add an event listener to the chat box for detecting manual scrolling
chatBox.addEventListener('scroll', () => {
    // Calculate the maximum scroll position that is considered "at the bottom"
    const maxScrollAtBottom = chatBox.scrollHeight - chatBox.clientHeight;
    // Check if the user is at the bottom of the chat box or near it
    shouldScrollToBottom = chatBox.scrollTop >= maxScrollAtBottom;
});
// Function to periodically retrieve and display chat messages
function displayChat() {
    // Fetch chat messages from the server using the /read endpoint
    fetch(backendUrl + '/read', {
        method: "GET",
    })
        .then((response) => response.json())
        .then((data) => {
            // Get the current scroll position
            const prevScrollTop = chatBox.scrollTop;
            // Clear the chat box before displaying new messages
            chatBox.innerHTML = "";
            // Display each new message in the chat box in reverse order
            for (let i = Math.max(data.length - 50, 0); i < data.length; i++) {
                const messageElement = document.createElement("div");
                messageElement.textContent = data[i].message;
                chatBox.appendChild(messageElement);
            }
            // If the user was at the bottom before new messages, scroll to the bottom
            if (shouldScrollToBottom) {
                scrollToBottomIfAtBottom();
            } else {
                // If the user was not at the bottom, maintain the scroll position
                chatBox.scrollTop = prevScrollTop;
            }
        })
        .catch((error) => {
            console.error("Failed to retrieve chat messages:", error);
        });
}
// Retrieve and display chat messages initially and every few seconds
displayChat();
setInterval(displayChat, 200); // Update the chat every 2 seconds
    </script>
    </body>
</html>
