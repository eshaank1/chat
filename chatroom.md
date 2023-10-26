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
            background-color: #2a0134;
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
            border-bottom: 1px solid #000;
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
        background-color: #FFF; /* Change background color for sent messages in light mode */
        border-radius: 5px;
        margin: 5px 0;
        padding: 10px;
        word-wrap: break-word;
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
        #emojiButton {
        background-color: #ADD8E6; /* Change emoji button color to blue in light mode */
    }
    </style>
</head>
<body>
    <div class="chatroom">
        <div class="chatroom-header">
            <h2>Chatroom</h2>
            <button id="toggleModeButton" onclick="toggleMode()">Toggle Mode</button>
        </div>
        <div class="chatroom-messages" id="chatroom-messages">
            <!-- Messages displayed here -->
        </div>
        <div class="chatroom-input">
            <input type="text" id="user-input" placeholder="Type your message...">
            <button id="emojiButton">Select Emoji</button> <!-- Added emoji button -->
            <button id="myBtn" onclick="sendMessage()">Send</button>
        </div>
    </div>
    <!-- JavaScript for sending messages, toggling dark/light mode, and emoji selection -->
    <script>
        var input = document.getElementById("user-input");
        input.addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                event.preventDefault();
                document.getElementById("myBtn").click();
            }
        });
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
        function toggleMode() {
            const body = document.body;
            const chatroom = document.querySelector('.chatroom');
            const chatroomHeader = document.querySelector('.chatroom-header');
            const chatroomMessages = document.querySelector('.chatroom-messages');
            const input = document.querySelector('input[type="text"]');
            const button = document.querySelector('button#myBtn');
            const toggleButton = document.querySelector('#toggleModeButton');
            if (body.classList.contains('dark-mode')) {
                body.classList.remove('dark-mode');
                chatroom.style.backgroundColor = '#FFF'; // Light mode background color
                chatroomHeader.style.backgroundColor = '#ADD8E6';
                chatroomHeader.style.color = '#ADD8E6';
                chatroomMessages.style.backgroundColor = '#FFF';
                input.style.backgroundColor = '#ADD8E6';
                input.style.color = '#FFFFFF';
                button.style.backgroundColor = '#ADD8E6';
                button.style.color = '#FFFFFF';
                toggleButton.textContent = 'Dark Mode';
            } else {
                body.classList.add('dark-mode');
                chatroom.style.backgroundColor = '#000'; // Dark mode background color
                chatroomHeader.style.backgroundColor = '#301934';
                chatroomHeader.style.color = '#000';
                chatroomMessages.style.backgroundColor = '#000';
                input.style.backgroundColor = '#301934';
                input.style.color = '#FFFFFF';
                button.style.backgroundColor = '#301934';
                button.style.color = '#FFFFFF';
                toggleButton.textContent = 'Light Mode';
            }
        }
        // Function for opening an emoji picker
        function selectEmoji() {
            // Use the "emoji-button" library to create an emoji picker
            const picker = new EmojiButton();
            picker.on('emoji', emoji => {
                const userInput = document.getElementById('user-input');
                userInput.value += emoji; // Insert the selected emoji into the input field.
            });
            // Show the emoji picker
            picker.showPicker(document.getElementById('emojiButton'));
        }
        // Event listener for the emoji button
        const emojiButton = document.getElementById('emojiButton');
        emojiButton.addEventListener('click', selectEmoji);
    </script>
    <!-- Include the emoji-button library -->
    <script src="https://cdn.jsdelivr.net/npm/emoji-button@3.0.0/dist/emoji-button.min.js"></script>
</body>
</html>