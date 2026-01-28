---
layout: false
comment: false
title: 404
permalink: /404
date: 2025-02-02 12:33:09
---

<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>404 Not Found</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Georgia', serif;
            background-color: #f4f4f4;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            text-align: center;
            flex-direction: column;
            padding: 20px;
        }
        h2 {
            font-size: 2rem;
            margin-bottom: 20px;
            color: #6A4E39;
            font-style: italic;
        }
        img {
            max-width: 100%;
            margin-bottom: 30px;
            border: 5px solid #6A4E39;
            border-radius: 10px;
        }
        #hitokoto {
            font-size: 1.5rem;
            color: #6A4E39;
            font-family: 'Times New Roman', serif;
            font-style: italic;
        }
        #hitokoto_text {
            font-size: 1.5rem;
            color: #4E6A60;
            text-decoration: none;
            font-weight: bold;
            transition: color 0.3s ease;
        }
        #hitokoto_text:hover {
            color: #6A4E39;
        }
        .container {
            text-align: center;
            padding: 50px 60px;
            border-radius: 15px;
            background-color: #fff8f1;
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
            border: 3px solid #6A4E39;
            max-width: 800px;
            width: 90%;
        }
        .container p {
            font-family: 'Arial', sans-serif;
            font-size: 0.8rem;
            margin-top: 20px;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>It looks like you're lost...</h2>
        <img src="https://http.cat/404" alt="404 Cat">
        <p id="hitokoto">
            <a href="#" id="hitokoto_text">:D 获取中...</a>
        </p>
    </div>
</body>
<script src="https://v1.hitokoto.cn/?encode=js&select=%23hitokoto" defer></script>
</html>

