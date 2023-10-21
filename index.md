---
layout: default
title: Student Blog
---

<html>
<head>
  <style>
    .welcome-background {
      width: 600px;
      height: 100px;
      background-color: red;
      animation-name: example;
      animation-duration: 4s;
      animation-iteration-count: infinite;
    }
        @keyframes example {
      0%   {background-color: red;}
      25%  {background-color: yellow;}
      50%  {background-color: blue;}
      100% {background-color: green;}
    }
    .welcome-text {
      font-family: "Lucida Console", "Courier New", monospace;
            text-align: center;
            line-height: 50px;
            font-size: 50px;
            text-shadow: 2px 2px 5px DarkOrchid;
    }
  </style>
</head>
<body>
  <div class='welcome-background'>
  <div class='welcome-text'>
    <p>Welcome to Chat Room</p>
    <br>
    <br>
  </div>

<html>
<head>
  <style>
    .styled-button {
      display: inline-block;
      padding: 10px 20px;
      background-color: #3498db; /* Change the background color as desired */
      color: #fff; /* Change the text color as desired */
      text-decoration: none;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
      transition: background-color 0.3s;
    }
    .styled-button:hover {
      background-color: #2980b9; /* Change the hover background color as desired */
    }
  </style>
</head>
<body>
  <a class="styled-button" href="{{site.baseurl}}/chatroom/">CHATROOM</a>
  <br>
  <br>
  <br>
</body>
</html>
<html>
<head>
    <style>
        .p1 {
            font-family: "Lucida Console", "Courier New", monospace;
            text-align: center;
            line-height: 50px;
            font-size: 30px;
            text-shadow: 2px 2px 5px blue;
            color: white
          }
        .p2 {
          font-family: "Lucida Console", "Courier New", monospace;
          text-align: center;
          line-height: 20px;
          font-size: 15px;
          color: white
        }
      </style>
</head>
<body>
    <div class='p1'>
    <p>How This Website Works?</p>
    </div>
    <div class='p2'>
    <i>In this chat website, we make a server that allow users to go inside and chat in  our website. In our website, people can set a username so next time they join, they will got their name. </i>
    </div>
</body>