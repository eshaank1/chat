---
comments: false
layout: default
title: Discussion Board
permalink: /discussionboard
---

## Discussion Board

%%html
<html>
<head>
    <title>Discussion Board</title>
    <style>
        /* Basic styling for the discussion board */
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        #discussion-list {
            list-style-type: none;
            padding: 0;
        }
        .discussion-item {
            border: 1px solid #ccc;
            padding: 10px;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <h1>Discussion Board</h1>
    <!-- Create a new discussion form -->
    <h2>Create a New Discussion</h2>
    <form id="create-discussion-form">
        <input type="text" id="discussion-title" placeholder="Discussion Title">
        <button type="submit">Create</button>
    </form>
    <!-- List of discussions -->
    <h2>Discussions</h2>
    <ul id="discussion-list"></ul>
    <!-- JavaScript to interact with the API -->
    <script>
        // Function to fetch and display discussions
        function fetchDiscussions() {
            fetch('/discussions', { method: 'GET' })
                .then(response => response.json())
                .then(data => {
                    const discussionList = document.getElementById('discussion-list');
                    discussionList.innerHTML = '';
                    data.forEach(discussion => {
                        const item = document.createElement('li');
                        item.className = 'discussion-item';
                        item.innerHTML = discussion.title;
                        discussionList.appendChild(item);
                    });
                });
        }
        // Function to create a new discussion
        document.getElementById('create-discussion-form').addEventListener('submit', function (e) {
            e.preventDefault();
            const discussionTitle = document.getElementById('discussion-title').value;
            fetch('/discussions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ 'title': discussionTitle }),
            })
            .then(() => {
                fetchDiscussions();  // Refresh the discussion list after creating a new discussion
                document.getElementById('discussion-title').value = '';
            });
        });
        // Initial fetch of discussions
        fetchDiscussions();
    </script>
</body>
</html>