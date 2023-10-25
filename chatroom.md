---
comments: false
layout: default
title: Chatroom
permalink: /chatroom
---

## chat room here

<html>
<head>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #2a0134;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chatroom {
            width: 700px;
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
            border-bottom: 1px solid #000;
        }
        .chatroom-messages {
            min-height: 200px;
            padding: 10px;
            overflow-y: auto;
            background-color: #000;
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
            <h2>Chatroom</h2>
        </div>
        <div class="chatroom-messages" id="chatroom-messages">
            <!-- Messages will be displayed here -->
        </div>
        <div class="chatroom-input">
            <input type="text" id="user-input" placeholder="Type your message...">
            <button id="myBtn" onclick="sendMessage()">Send</button>
            <script>
            var input = document.getElementById("user-input");
            input.addEventListener("keypress", function(event) {
                if (event.key === "Enter") {
                    event.preventDefault();
                    document.getElementById("myBtn").click();
                }
            });
            </script>
        </div>
    </div>
    <script>
        function sendMessage() {
            const userInput = document.getElementById('user-input');
            const message = userInput.value.trim();
            if (message !== '') {
                const chatroomMessages = document.getElementById('chatroom-messages');
                const messageElement = document.createElement('div');
                messageElement.textContent = message;
                chatroomMessages.appendChild(messageElement);
                userInput.value = '';
            }
        }
    </script>
</body>
</html>

<html>
<head>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #2a0134;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .chatroom {
            width: 700px;
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
            border-bottom: 1px solid #000;
        }
        .chatroom-messages {
            min-height: 200px;
            padding: 10px;
            overflow-y: auto;
            background-color: #000;
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
            <h2>Chatroom</h2>
        </div>
        <div class="chatroom-messages" id="chatroom-messages">
            <!-- Messages will be displayed here -->
        </div>
        <div class="chatroom-input">
            <input type="text" id="user-input" placeholder="Type your message...">
            <button id="myBtn" onclick="sendMessage()">Send</button>
            <button id="themeBtn" onclick="toggleTheme()">Change Theme</button>
            <script>
            var input = document.getElementById("user-input");
            input.addEventListener("keypress", function(event) {
                if (event.key === "Enter") {
                    event.preventDefault();
                    document.getElementById("myBtn").click();
                }
            });
            // Function to toggle between light and dark themes
            function toggleTheme() {
                const body = document.body;
                if (body.classList.contains("dark-theme")) {
                    body.classList.remove("dark-theme");
                } else {
                    body.classList.add("dark-theme");
                }
            }
            </script>
        </div>
    </div>
    <script>
        function sendMessage() {
            const userInput = document.getElementById('user-input');
            const message = userInput.value.trim();
            if (message !== '') {
                const chatroomMessages = document.getElementById('chatroom-messages');
                const messageElement = document.createElement('div');
                messageElement.textContent = message;
                chatroomMessages.appendChild(messageElement);
                userInput.value = '';
            }
        }
    </script>
</body>
</html>
