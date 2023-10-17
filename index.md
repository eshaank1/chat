---
layout: default
title: Student Blog
---


## Welcome To The Chat Room

Currently under development :)

# Here's a button while you wait

<!DOCTYPE html>
<html>
<head>
<style>
  #movingButton {
    position: absolute;
    top: 50px;
    left: 50px;
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

