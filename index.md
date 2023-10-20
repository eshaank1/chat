---
layout: default
title: Student Blog
---


## Welcome To The Chat Room

Currently under development :)

# Here's a button while you wait

<html>
<head>
<style>
  #movingButton {
    position: absolute;
    top: 100px;
    left: 100px;
  }
</style>
</head>
<body>

<button id="movingButton">Click Me</button>

<script>
  const button = document.getElementById("movingButton");
  const maxX = window.innerWidth - button.clientWidth;
  const maxY = window.innerHeight - button.clientHeight;

  button.addEventListener("click", () => {
    const newX = Math.random() * maxX;
    const newY = Math.random() * maxY;

    button.style.left = newX + "px";
    button.style.top = newY + "px";
  });
</script>

</body>
</html>

<html>
<head>
    <style>
        .box {
            width: 600px;
            height: 100px;
            background-color: white;
            color: black;
            position: relative;
        }
        .p1 {
            font-family: "Lucida Console", "Courier New", monospace;
            text-align: center;
            line-height: 50px;
            font-size: 30px;
            text-shadow: 2px 2px 5px lightblue;
          }
        .p2 {
          font-family: "Lucida Console", "Courier New", monospace;
            text-align: center;
            line-height: 30px;
            font-size: 15px;
        }
      </style>
</head>
<body>
    <div class='p1'>
    <div class='box'>
    <p>How This Website Works?</p>
    </div>
    <div class='p2'>
    <div class='box'>
    <i>In this chat website, we make a server that allow users to go inside and chat in  our website. In our website, people can set a username so next time they join, they will got their name. </i>
    </div>
