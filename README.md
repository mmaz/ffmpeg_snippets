# ffmpeg_snippets

## Shrink and trim

bump up the CRF for smaller files:

```
ffmpeg -ss 00:00:00 -t 00:03:00 -i "input.mp4" -c:v libx264 -preset slow -crf 24 -an -pix_fmt yuv420p output_small.mp4
```

## Cropping

https://video.stackexchange.com/a/4571

```bash
ffmpeg -i in.mp4 -filter:v "crop=out_w:out_h:x:y" out.mp4
-filter:v "crop=700:700:0:60" out.mp4 # for example
# To crop the bottom right quarter
ffmpeg -i in.mp4 -filter:v "crop=in_w/2:in_h/2:in_w/2:in_h/2" -c:a copy out.mp4
# equivalently:
ffmpeg -i in.mp4 -filter:v "crop=320/2:240/2:320/2:240/2" -c:a copy out.mp4
```

## [video from images](https://stackoverflow.com/questions/24961127/how-to-create-a-video-from-images-with-ffmpeg)

60FPS: 

```
ffmpeg -framerate 60 -pattern_type glob -i '*.png' -c:v libx264 -pix_fmt yuv420p out.mp4
```

## [Add text](https://stackoverflow.com/a/17624103)

```
ffmpeg -i input.mp4 -vf drawtext="/usr/share/fonts/truetype/ubuntu/Ubuntu-B.ttf: \
text='Stack Overflow': fontcolor=white: fontsize=24: box=1: boxcolor=black@0.5: \
boxborderw=5: x=(w-text_w)/2: y=(h-text_h)/2" -codec:a copy output.mp4
```

# Screen recording

## Gnome screenshot

`Shift` + `Print Screen` for a portion of the window

## Gnome Screen Recorder

`Ctrl` + `Shift` + `Alt` + `R` to start and stop

Change default length (`sudo` **not** necessary):

```bash
gsettings set org.gnome.settings-daemon.plugins.media-keys max-screencast-length 45
```

* [Ubuntu docs](https://help.ubuntu.com/stable/ubuntu-help/screen-shot-record.html)
* [`gsettings` tip from here](http://antoine-schellenberger.com/linux/2014/11/03/change-default-screencast-duration-in-gnome-3.html)
