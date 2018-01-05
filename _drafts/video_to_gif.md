С качественным изображением https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality

ffmpeg -i ezgif.com-video-to-gif.webm -vf fps=10,scale=1024:-1:flags=lanczos,palettegen palette.png
ffmpeg -i ezgif.com-video-to-gif.webm -i palette.png -filter_complex "fps=10,scale=1024:-1:flags=lanczos[x];[x][1:v]paletteuse" output.gif

https://ezgif.com/video-to-gif
