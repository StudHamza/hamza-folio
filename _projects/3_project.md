---
layout: page
title: Treasure Hunt Website
description: A fully fledged website written in python django, i containes a QR code system to operate a treasure hunt game. Hosted on Azure
img: assets/img/zfetch.png
importance: 3
category: personal
---

Using Django, Mysql and Azure I created a treasure hunt game with built in QR code scanning. <a href='redirect: https://treasure-hunt-irl.azurewebsites.net/'>Zfetch</a> Sadly I lost this project, since it was hosted on Azure app services free plan that was changed in May 2024.

The website had features like, create event, edit event for admins and join event for users. The encoded each puzzle peice with a QR code, only by scanning the code can you reveal the riddle asscoiated with it, this riddle tells the place of the next code and finally the treasure.

Using websocket, their was a integrated chat system and annoucment system, if a riddle is not found in a certain amount of time the riddle shown to everyone. And if somone finds a riddle, the riddle will be announce 1 hour after the finding as a advantage to the riddle finder.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/zfetch.png" title="Scraped Website" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Zfecth Treasure Hunt Game
</div>