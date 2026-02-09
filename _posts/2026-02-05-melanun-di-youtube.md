---
title: 'Melanun Di Youtube'
date: 2026-02-05 21:09:05
updated_at: 2026-02-05 21:09:09
tags:
- tutorial
---

Menulis ini sebagai nota takut hilang ilmu lanun yang berharga ni.

---

- [Audio MP3 highest quality](#audio-mp3-highest-quality)
  - [Single](#single)
  - [Batch download](#batch-download)
- [Cut duration](#cut-duration)
- [Video highest quality](#video-highest-quality)
- [Download a playlist](#download-a-playlist)
- [Cut video](#cut-video)

---

Download *yt-dlp* executable from [Github](https://github.com/yt-dlp/yt-dlp/releases).

#### Audio MP3 highest quality

##### Single

```shell
./yt-dlp.exe --extract-audio --output "%(title)s.%(ext)s" --audio-format mp3 --audio-quality 0 INSERT_URL
```

##### Batch download

```shell
./yt-dlp.exe --extract-audio --output "%(title)s.%(ext)s" --audio-format mp3 --audio-quality 0 -ci --batch-file=batch.txt
```

#### Cut duration

```shell
./yt-dlp.exe --extract-audio --output "%(title)s.%(ext)s" --audio-format mp3 --audio-quality 0 INSERT_URL --downloader ffmpeg --downloader-args "ffmpeg_i:-ss 00:10:07"
```

Example above, *start at 10 minutes 7 seconds*

Use `ffmpeg_i:-ss 00:00:10.00 -t 00:00:30.00` to download only a specific duration (e.g., 30 seconds from the start time). `-t` specifies the duration of the resulting file, not an end time. Apa-apa, Google je.

#### Video highest quality

```shell
./yt-dlp.exe -f 'bestvideo[ext=mp4]+bestaudio[ext=m4a]/bestvideo+bestaudio' --merge-output-format mp4 INSERT_URL
```

#### Download a playlist

```shell
./yt-dlp.exe -i -f mp4 --yes-playlist PLAYLIST_URL
```

#### Cut video

```shell
./ffmpeg.exe -ss 262.2 -t 56.3 -i VIDEO_FILE.EXT -y -c copy OUTPUT_VIDEO.mp4
```

More info: https://ytcutter.cc/

- `-ss`: set the start time ofkfset
- `-t`: record or transcode "duration" seconds of audio/video

NOTE: use yt-dlp.exe instead
