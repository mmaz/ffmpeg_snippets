# ffmpeg_snippets

## Shrink filesize and trim

bump up the CRF for smaller files:

```
ffmpeg -ss 00:00:00 -t 00:03:00 -i "input.mp4" -c:v libx264 -preset slow -crf 24 -an -pix_fmt yuv420p output_small.mp4
```

## Reduce resolution

```
 -vf scale=640:360         # exactly
 -vf "scale=iw*0.8:ih*0.8" # by percentage
```

[More details here](https://trac.ffmpeg.org/wiki/Scaling)

## Change playback speed

Trim video, and slow video down by 7x [[wiki](https://trac.ffmpeg.org/wiki/How%20to%20speed%20up%20/%20slow%20down%20a%20video)]

```bash
fmpeg -ss 00:00:00 -t 00:00:10 -i input.mp4 -c:v libx264 -filter:v "setpts=7*PTS" -preset slow -crf 26 -an -pix_fmt yuv420p output.mp4
```

## Make gif: [link](https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality)

`setpts` speeds up video, `scale` for width, and `-loop 0` for infinite looping.

*todo:* chaining in a speedup command, e.g., `-vf "setpts=0.3*PTS,fps=10,...` does not seem to play well with looping.

```bash
ffmpeg -i input.mp4 -vf "fps=10,scale=400:-1:flags=lanczos,split[s0][s1];[s0]palettegen[p];[s1][p]paletteuse" -loop 0 output.gif
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

## video from images: [link](https://stackoverflow.com/questions/24961127/how-to-create-a-video-from-images-with-ffmpeg)

60FPS: 

```
ffmpeg -framerate 60 -pattern_type glob -i '*.png' -c:v libx264 -pix_fmt yuv420p out.mp4
```

## add text: [link](https://stackoverflow.com/a/17624103)

**note:** text can't contain `:` (at least, not unescaped) 

```
ffmpeg -i input.mp4 -vf drawtext="/usr/share/fonts/truetype/ubuntu/Ubuntu-B.ttf: \
text='example': fontcolor=white: fontsize=24: box=1: boxcolor=black@0.5: \
boxborderw=5: x=(w-text_w)/2: y=(h-text_h)/2" -codec:a copy output.mp4
```

## chain image sequence and text

```bash
ffmpeg -framerate 10 -pattern_type glob -i '*.png' \
-vf drawtext="/usr/share/fonts/truetype/ubuntu/Ubuntu-B.ttf: \
text='hello world': fontcolor=white: fontsize=18: box=1: boxcolor=black@0.5: \
boxborderw=5: x=(w-text_w)/2: y=(h-text_h)/8" -c:v libx264  -preset veryslow -crf 22  -pix_fmt yuv420p ../videoname.mp4
```

## chain cropping and text: [link](https://trac.ffmpeg.org/wiki/FilteringGuide#FiltergraphChainFilterrelationship)

Separate filters in a -vf chain with a `,`: 

```bash
ffmpeg -i input.webm \
-vf crop=809:616:37:63,drawtext="/usr/share/fonts/truetype/ubuntu/Ubuntu-B.ttf: \
text='My Text': fontcolor=white: fontsize=18: box=1: boxcolor=black@0.5: \
boxborderw=5: x=(w-text_w)/2: y=(h-text_h)/8" \
-c:v libx264  -preset veryslow -crf 22  -pix_fmt yuv420p out.mp4
```

# Screen recording

## `ffmpeg`

```
ffmpeg -video_size 1024x768 -framerate 10 -f x11grab -i :1.0+0,0 -c:v libx264 -an -pix_fmt yuv420p output.mp4
```

* `:1` is the display ID
* Width and height are determined by `-video_size`
* Offset is determined by `+x,y`
* [Docs](https://trac.ffmpeg.org/wiki/Capture/Desktop)

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

## Simple Screen recorder

* [homepage](https://www.maartenbaert.be/simplescreenrecorder/)
* [github](https://github.com/MaartenBaert/ssr)
