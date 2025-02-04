---
layout: page
title: Money printing machine
description: Python script that uses the reddit API to fetch hot stories for r/stories, then uses google cloud text to speech to create a subtitle.srt file, which is then added to a random satsfing video.  
img: assets/img/reddit_yt.png
importance: 1
category: software
related_publications: true
---

Tiktok motization program is a gold rush if used right. One of the most popular videos on tiktok and other short form social media 

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/google_text_to_speech.png" title="Google API" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/reddit_yt.png" title="Final Product" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Google API voices, and Final Product
</div>
For video processing and subtitle overlay I used subprocess library to use the ffmpeg command in python.

This is the final product. Note that all of the imports are local files, this way i modularized the script and will be able to edit and modify it sometime in the future.
{% raw %}

```python
from reddit import get_stories
import constants as c
from google_api import text_to_wav, transcribe_audio, write_srt_file
from subtitle import *
from ffmpeg import add_subtitles_audio


stories,titles = get_stories()

story = titles[1]+'\n'+stories[1]

text_to_wav(c.VOICE_NAME,story,file_name="story.wav")


results = transcribe_audio("story.wav")

write_srt_file(results, "subtitle.srt")

story_duration = get_audio_duration("story.wav")
letter_duration = story_duration/len(story)
title_duration = len(titles[0])*letter_duration

```

{% endraw %}
