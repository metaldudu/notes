
## 转换格式

```
ffmpeg -i "1.webm" -vn -ab 128k -ar 44100 -y "1.mp3"
```

###  m4a/webm to mp3

ffmpeg -i input.m4a -c:v copy -c:a libmp3lame -q:a 2 output.mp3

ffmpeg -i input.webm -c:v copy -c:a libmp3lame -q:a 2 output.mp3

ffmpeg -i in.mp4 -c:v copy -c:a libmp3lame -q:a 2 out.mp3

ffmpeg -i input.wav -vn -ar 44100 -ac 2 -b:a 192k output.mp3


### 批量转换m4a到mp3

```
$ mkdir newfiles
$ for f in *.m4a; do ffmpeg -i "$f" -codec:v copy -codec:a libmp3lame -q:a 2 newfiles/"${f%.m4a}.mp3"; done
```

一行的例子

```
for i in *.m4a; do ffmpeg -i "$i"  -c:v copy -c:a libmp3lame -q:a 4 "${i%.*}.mp3"; done
```

### wav to flac

```
ffmpeg -i in.wav  out.flac
```

批量wav到flac

```
for i in *.ape; do ffmpeg -i "$i" "${i%.*}.flac"; done
```


### 下载HLS在线视频： 

```
ffmpeg -i "https://www3.yuboyun.com/hls/2018/11/05/KCEwXYoY/playlist.m3u8" -codec copy 1.ts
```

###  b站flv转mp4

`ffmpeg -i S02E06.flv -c copy -copyts S02E06.mp4`


### 合并mp4

```
ffmpeg -f concat -i mylist.txt -c copy output.mp4
```

其中 mylist.txt 格式是

```

file '01.mp4'
file '02.mp4'
```

### 批量合并mp3脚本

```bash
ffmpeg \
-f concat \
-safe 0 \
-i <(find "$(pwd)" -iname '*.mp3' -printf "file '%p'\n" | sort) \
-c copy \
merged.mp3
```
### gif to mp4

http://note.rpsh.net/posts/2015/04/21/mac-osx-ffmpeg-mp4-gif-convert/

简单的命令是：

```
ffmpeg -f gif -i animation.gif animation.mp4
```

但微信不能播放，找到另外一个方法：

```
ffmpeg -i animation.gif -movflags faststart -pix_fmt yuv420p -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2" animation.mp4
```

参数应该是强制mp4的压缩格式、画面比例

### 添加硬字幕

```
ffmpeg -i mymovie.mp4 -vf subtitles=subtitles.srt mysubtitledmovie.mp4
```

### 去开头

批量去掉开头广告，16秒

````bash
#! /bin/bash

for i in *.mp4; do
  ffmpeg -ss 00:00:16 -i $i -c:v copy -c:a copy ~/tmp/$i
done
````

### 下载m3u

```
ffmpeg -headers "referer: https://emturbovid.com/" -i <url> -c copy -bsf:a aac_adtstoasc output_file.mp4
```

### 减小视频体积

```
ffmpeg -i input.mp4 -vcodec libx264 -crf 20 output.mp4
```

## 参考

- [ffmpeg 入门笔记](http://einverne.github.io/post/2015/12/ffmpeg-first.html)