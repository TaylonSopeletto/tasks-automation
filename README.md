## Download and convert youtube video to mp3

```python
import os
from pytube import YouTube
from moviepy.editor import *
from pathlib import Path

# get current directory
directory = os.getcwd()

# create folder if not exists
Path(directory + '/songs').mkdir(parents=True, exist_ok=True)


# read songs list and run each line
with open('songs.txt') as f:
    lines = f.readlines()
    for x in lines:

        # find video filtering mp4 and get first option
        yt = YouTube(x)
        stream = yt.streams.filter(file_extension='mp4')
        stream = yt.streams.get_by_itag(stream[0].itag)
        stream.download()

        # set files diretory
        mp4_file = directory + "/" + yt.title + ".mp4"
        mp3_file = directory + "/songs/" + yt.title + ".mp3"

        # convert mp4 to mp3
        videoclip = VideoFileClip(mp4_file)
        audioclip = videoclip.audio
        audioclip.write_audiofile(mp3_file)
        audioclip.close()
        videoclip.close()

        test = os.listdir(directory)

        for item in test:
            if item.endswith(".mp4"):
                os.remove(os.path.join(directory, item))
        
    


```