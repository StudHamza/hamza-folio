---
layout: page
title: Anki Deck Automation
description: Using beautiful soup, gTTS (Google Text-to-Speech) and genanki python libraries, I created a script that scrapes a japanese website to create a anki deck for the book みんなの日本語.
img: assets/img/anki_deck_jap.png
importance: 2
category: personal
giscus_comments: false
---

This projects is one of the coolest projects since it really benifited me, 4 months in, until now.

Using Inspect in chrome, I was able to extract the general layout of the website; this did help filter on the text I needed when I wrote the web scraping script.
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/japanese_website.png" title="Scraped Website" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Japanese Website
</div>
The scrapped data is then saved a json file and processes in a while loop using the gTTs library. Each word is then searched in "じしょう" or jishio. Which is the japanese dictionary. Finnaly the final product looks like this:

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/japanese_anki.jpg" title="Anki Deck" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/anki_deck_jap.png" title="Anki Deck detail" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The anki deck, Still use it after my 4 months japanese journey.
</div>

{% raw %}


{% endraw %}
