# ffmpeg_snippets

* Shrink and trim

bump up the CRF for smaller files:

```
ffmpeg -ss 00:00:00 -t 00:03:00 -i "input.mp4" -c:v libx264 -preset slow -crf 24 -an -pix_fmt yuv420p output_small.mp4
```

## Gnome screenshot

`Shift` + `Print Screen` for a portion of the window

## Gnome Screen Recorder

`Ctrl` + `Shift` + `Alt` + `R` to start and stop

Change default length:

```bash
gsettings set org.gnome.settings-daemon.plugins.media-keys max-screencast-length 45
```

* [Ubuntu docs](https://help.ubuntu.com/stable/ubuntu-help/screen-shot-record.html)
* [`gsettings` tip from here](http://antoine-schellenberger.com/linux/2014/11/03/change-default-screencast-duration-in-gnome-3.html)
