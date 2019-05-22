# ffmpeg_snippets

* Shrink and trim

bump up the CRF for smaller files:

```
ffmpeg -ss 00:00:00 -t 00:03:00 -i "input.mp4" -c:v libx264 -preset slow -crf 24 -an -pix_fmt yuv420p output_small.mp4
```
