FFmpeg Commands
===============

Show Streams and Parameters
###########################

.. code-block:: bash

    ffprobe -i input.mkv

Extract Streams: Subtitles, Audio, Video
########################################

Extract Subtitles
*****************

.. code-block:: bash

    ffmpeg -i input.mkv -map 0:s:0 subs.srt


Extract Audio
*************

.. code-block:: bash

    ffmpeg -i input.mkv -vn -acodec copy output-audio.aac

- ``-vn`` is no video.
- ``-acodec`` copy says use the same audio stream that's already in there.

More info & source: https://stackoverflow.com/questions/9913032/how-can-i-extract-audio-from-video-with-ffmpeg


Hardcoding a subtitle in a video
################################

.. code-block:: bash

    ffmpeg -i input.mkv -vf "subtitles=subtitle.srt" -c:v libx264 -crf 20 -c:a aac -b:a 192k output.mp4
    # OR
    ffmpeg -i input.mp4 -vf "subtitles=subtitle.srt" output.mp4


To change the font, directly edit the parameters in the srt/ass subtitle file.


Source: https://superuser.com/questions/869248/hardcoding-subs-with-ffmpeg


Images to Video
###############

.. code-block:: bash

    ffmpeg -framerate 30 -i img_%05d.png video_manual.mp4


Video to GIF
############

.. code-block:: bash

    ffmpeg -ss 61.0 -t 2.5 -i input.mp4 -filter_complex "[0:v] palettegen" palette.png
    ffmpeg -ss 61.0 -t 2.5 -i input.mp4 -i palette.png -filter_complex "[0:v][1:v] paletteuse" output.gif


:code:`-ss 61.0 -t 2.5`: Processing only 2.5 seconds starting from time 61 seconds


ffmpeg -i Movie.mkv -map 0:s:0 subs.srt


------------------------------------------------------------

**Sources**:

- Video to GIF: https://engineering.giphy.com/how-to-make-gifs-with-ffmpeg/
- Extract subtitles: https://superuser.com/questions/583393/how-to-extract-subtitle-from-video-using-ffmpeg
- Extract audio: https://stackoverflow.com/questions/9913032/how-can-i-extract-audio-from-video-with-ffmpeg
- Hardcoding subtitle: https://superuser.com/questions/869248/hardcoding-subs-with-ffmpeg
