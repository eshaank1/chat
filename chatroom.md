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
        <div class="chatroom-messages" id="chatroom-messages">
            <!-- Messages will be displayed here -->
        </div>
        <div class="chatroom-input">
            <input type="text" id="message" placeholder="Type your message" onkeypress="handleKeyPress(event)">
            <button id="send" onclick="sendMessage()">Send</button>
            <button id="toggleModeButton" onclick="toggleMode()">Toggle Mode</button>
            <button id="moodCheck">Neutral Mood</button>
        </div>
    </div>
    <script>
        const chatBox = document.getElementById("chatroom-messages");
        const messageInput = document.getElementById("message");
        const backendUrl = "https://chat.stu.nighthawkcodingsociety.com/api/chats";
        const badMood = ['trash', 'bad', 'sucks', 'suck', 'stupid', 'jerk'];
        const goodMood = ['like', 'good', 'happy', 'amazing', 'great', 'haha'];
        function sendMessage() {
            const message = messageInput.value.trim();
            if (message !== '') {
                const timestamp = new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                const messageWithTimestamp = `[${timestamp}] ${message}`;
                const messageElement = document.createElement("div");
                messageElement.textContent = messageWithTimestamp;
                // Move all existing messages up one position
                const messages = chatBox.querySelectorAll("div");
                for (let i = messages.length - 1; i >= 0; i--) {
                    if (i === 0) {
                        chatBox.removeChild(messages[i]);
                    } else {
                        messages[i].textContent = messages[i - 1].textContent;
                    }
                }
                // Add the new message at the top
                chatBox.appendChild(messageElement);
                document.getElementById("message").value = "";
                fetch(backendUrl + '/create', {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json",
                    },
                    body: JSON.stringify({ message: messageWithTimestamp }),
                })
                .then((response) => {
                    if (response.status === 200) {
                        messageInput.value = '';
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
        function displayChat() {
            fetch(backendUrl + '/read', {
                method: "GET",
            })
            .then((response) => response.json())
            .then((data) => {
                // Clear the chat box
                chatBox.innerHTML = "";
                // Display each new message in the chat box
                for (let i = data.length - 1; i >= 0; i--) {
                    const messageElement = document.createElement("div");
                    messageElement.textContent = data[i].message;
                    chatBox.appendChild(messageElement);
                }
                var badMoodCount = 0;
                var goodMoodCount = 0;
                for (let i = data.length - 1; i >= 0; i--) {
                    for (let j = 0; j < badMood.length; j++) {
                       if(data[i].message.includes(badMood[j])) {
                          badMoodCount++;
                       }
                    }
                    for (let j = 0; j < goodMood.length; j++) {
                       if(data[i].message.includes(goodMood[j])) {
                          goodMoodCount++;
                       }
                    }
                    if (badMoodCount > goodMoodCount) {
                      const moodCheck = document.getElementById('moodCheck');
                      moodCheck.textContent = 'Bad Mood';
                    } else if (badMoodCount < goodMoodCount) {
                      const moodCheck = document.getElementById('moodCheck');
                      moodCheck.textContent = 'Good Mood';
                    } else {
                      const moodCheck = document.getElementById('moodCheck');
                      moodCheck.textContent = 'Neutral Mood';
                    }
                }
            })
            .catch((error) => {
                console.error("Failed to retrieve chat messages:", error);
            });
        }
        function toggleMode() {
            const body = document.body;
            const chatroom = document.querySelector('.chatroom');
            const chatroomHeader = document.querySelector('.chatroom-header');
            const chatroomMessages = document.querySelector('.chatroom-messages');
            const input = document.querySelector('input[type="text"]');
            const button1 = document.getElementById('send');
            const toggleButton = document.getElementById('toggleModeButton');
            const moodCheckButton = document.getElementById('moodCheck');
            if (body.classList.contains('dark-mode')) {
                body.classList.remove('dark-mode');
                chatroom.style.backgroundColor = '#FFF'; // Light mode background color
                chatroomHeader.style.backgroundColor = '#ADD8E6';
                chatroomHeader.style.color = '#000';
                chatroomMessages.style.backgroundColor = '#FFF';
                input.style.backgroundColor = '#ADD8E6';
                input.style.color = '#000';
                button1.style.backgroundColor = '#ADD8E6';
                button1.style.color = '#FFF';
                toggleButton.style.backgroundColor = '#ADD8E6';
                toggleButton.style.color = '#FFF';
                moodCheckButton.style.backgroundColor = '#ADD8E6';
                moodCheckButton.style.color = '#FFF';
                toggleButton.textContent = 'Dark Mode';
            } else {
                body.classList.add('dark-mode');
                chatroom.style.backgroundColor = '#000'; // Dark mode background color
                chatroomHeader.style.backgroundColor = '#301934';
                chatroomHeader.style.color = '#000';
                chatroomMessages.style.backgroundColor = '#000';
                input.style.backgroundColor = '#301934';
                input.style.color = '#FFF';
                button1.style.backgroundColor = '#301934';
                button1.style.color = '#FFF';
                toggleButton.style.backgroundColor = '#301934';
                toggleButton.style.color = '#FFF';
                moodCheckButton.style.backgroundColor = '#301934';
                moodCheckButton.style.color = '#FFF';
                toggleButton.textContent = 'Light Mode';
            }
        }   
        displayChat();
        setInterval(displayChat, 200);
    </script>
</body>
</html>
