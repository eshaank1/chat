---
comments: false
layout: default
title: Discussion Board
permalink: /discussionboard
---

<html>
<head>
    <title>Discussion Board</title>
    <style>
        /* Basic styling for the discussion board */
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        .discussion {
            border: 1px solid #ccc;
            padding: 10px;
            margin: 10px 0;
        }
        .post {
            border: 1px solid #eee;
            padding: 10px;
            margin: 10px 0;
        }
        .comment {
            border: 1px solid #f0f0f0;
            padding: 5px;
            margin: 5px 0;
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
    <div id="discussion-list"></div>
    <script>
        // Function to fetch and display discussions
        function fetchDiscussions() {
            fetch('/discussions', { method: 'GET' })
                .then(response => response.json())
                .then(data => {
                    const discussionList = document.getElementById('discussion-list');
                    discussionList.innerHTML = '';
                    data.forEach(discussion => {
                        // Create a discussion container
                        const discussionDiv = document.createElement('div');
                        discussionDiv.className = 'discussion';
                        // Add the discussion title
                        const title = document.createElement('h3');
                        title.innerText = discussion.title;
                        discussionDiv.appendChild(title);
                        // Add a form for creating a post
                        const postForm = document.createElement('form');
                        postForm.className = 'create-post-form';
                        postForm.innerHTML = `
                            <input type="text" placeholder="New Post">
                            <button type="submit">Post</button>
                        `;
                        // Add the post form to the discussion container
                        discussionDiv.appendChild(postForm);
                        // Add an empty div to display posts
                        const postContainer = document.createElement('div');
                        postContainer.className = 'post-container';
                        // Event listener to create a new post
                        postForm.addEventListener('submit', function (e) {
                            e.preventDefault();
                            const newPostInput = postForm.querySelector('input');
                            const newPost = newPostInput.value;
                            if (newPost) {
                                // Call the create post API and then refresh the discussion
                                createPost(discussion.title, newPost);
                                fetchDiscussions();
                                newPostInput.value = '';
                            }
                        });
                        // Append the post container to the discussion container
                        discussionDiv.appendChild(postContainer);
                        // Fetch and display posts for this discussion
                        fetchPosts(discussion.title, postContainer);
                        // Append the discussion container to the list
                        discussionList.appendChild(discussionDiv);
                    });
                });
        }
        // Function to create a new post
        function createPost(discussionTitle, content) {
            fetch(`/discussions?title=${discussionTitle}`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ 'content': content }),
            });
        }
        // Function to fetch and display posts for a discussion
        function fetchPosts(discussionTitle, postContainer) {
            fetch(`/discussions?title=${discussionTitle}`, { method: 'GET' })
                .then(response => response.json())
                .then(data => {
                    const posts = data[0].posts;
                    postContainer.innerHTML = '';
                    posts.forEach(post => {
                        // Create a post container
                        const postDiv = document.createElement('div');
                        postDiv.className = 'post';
                        // Add the post content
                        const content = document.createElement('p');
                        content.innerText = post.content;
                        postDiv.appendChild(content);
                        // Add a form for creating a comment
                        const commentForm = document.createElement('form');
                        commentForm.className = 'create-comment-form';
                        commentForm.innerHTML = `
                            <input type="text" placeholder="Leave a Comment">
                            <button type="submit">Comment</button>
                        `;
                        // Add the comment form to the post container
                        postDiv.appendChild(commentForm);
                        // Add an empty div to display comments
                        const commentContainer = document.createElement('div');
                        commentContainer.className = 'comment-container';
                        // Event listener to create a new comment
                        commentForm.addEventListener('submit', function (e) {
                            e.preventDefault();
                            const newCommentInput = commentForm.querySelector('input');
                            const newComment = newCommentInput.value;
                            if (newComment) {
                                // Call the create comment API and then refresh the comments
                                createComment(discussionTitle, post.id, newComment);
                                fetchComments(discussionTitle, post.id, commentContainer);
                                newCommentInput.value = '';
                            }
                        });
                        // Append the post container to the post container
                        postContainer.appendChild(postDiv);
                        // Append the comment container to the post container
                        postContainer.appendChild(commentContainer);
                        // Fetch and display comments for this post
                        fetchComments(discussionTitle, post.id, commentContainer);
                    });
                });
        }
        // Function to create a new comment
        function createComment(discussionTitle, postId, content) {
            fetch(`/discussions/${discussionTitle}/posts/${postId}/comments`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ 'content': content }),
            });
        }
        // Function to fetch and display comments for a post
        function fetchComments(discussionTitle, postId, commentContainer) {
            fetch(`/discussions/${discussionTitle}/posts/${postId}/comments`, { method: 'GET' })
                .then(response => response.json())
                .then(data => {
                    const comments = data.comments;
                    commentContainer.innerHTML = '';
                    comments.forEach(comment => {
                        // Create a comment container
                        const commentDiv = document.createElement('div');
                        commentDiv.className = 'comment';

                        // Add the comment content
                        const content = document.createElement('p');
                        content.innerText = comment.content;
                        commentDiv.appendChild(content);

                        // Append the comment container to the comment container
                        commentContainer.appendChild(commentDiv);
                    });
                });
        }
        // Initial fetch of discussions
        fetchDiscussions();
    </script>
</body>
</html>