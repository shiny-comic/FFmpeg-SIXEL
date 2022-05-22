FFmpeg README
=============

FFmpeg is a collection of libraries and tools to process multimedia content
such as audio, video, subtitles and related metadata.

## Libraries

* `libavcodec` provides implementation of a wider range of codecs.
* `libavformat` implements streaming protocols, container formats and basic I/O access.
* `libavutil` includes hashers, decompressors and miscellaneous utility functions.
* `libavfilter` provides means to alter decoded audio and video through a directed graph of connected filters.
* `libavdevice` provides an abstraction to access capture and playback devices.
* `libswresample` implements audio mixing and resampling routines.
* `libswscale` implements color conversion and scaling routines.

## Tools

* [ffmpeg](https://ffmpeg.org/ffmpeg.html) is a command line toolbox to
  manipulate, convert and stream multimedia content.
* [ffplay](https://ffmpeg.org/ffplay.html) is a minimalistic multimedia player.
* [ffprobe](https://ffmpeg.org/ffprobe.html) is a simple analysis tool to inspect
  multimedia content.
* Additional small tools such as `aviocat`, `ismindex` and `qt-faststart`.

## Documentation

* See the INSTALL file.

The offline documentation is available in the **doc/** directory.

The online documentation is available in the main [website](https://ffmpeg.org)
and in the [wiki](https://trac.ffmpeg.org).

### Examples

Coding examples are available in the **doc/examples** directory.

## License

FFmpeg codebase is mainly LGPL-licensed with optional components licensed under
GPL. Please refer to the LICENSE file for detailed information.

## Contributing

Patches should be submitted to the ffmpeg-devel mailing list using
`git format-patch` or `git send-email`. Github pull requests should be
avoided because they are not part of our review process and will be ignored.

SIXEL Integration
=============

[![ballmer](https://raw.githubusercontent.com/saitoha/FFmpeg-SIXEL/data/data/ballmer.png)](https://youtu.be/7z6lo4aq6zc)

## Step 0. Edit .Xresources

```
$ cat $HOME/.Xresources
XTerm*decTerminalID: vt340
XTerm*sixelScrolling: true
XTerm*regisScreenSize: 1920x1080
XTerm*numColorRegisters: 256

$ xrdb $HOME/.Xresources  # reload
```

## Step 1. Build xterm with --enable-sixel-graphics option

```
$ wget ftp://invisible-island.net/xterm/xterm.tar.gz
$ tar xvzf xterm.tar.gz
$ cd xterm-*
$ ./configure --enable-sixel-graphics
$ make
$ ./xterm  # launch
```

## Step 2. Build FFmpeg-SIXEL with libsixel
Note: libquvi support has been removed from ffmpeg.
To play youtube videos, you can use youtube-dl (or yt-dlp) to download a video, then you can play it with ffmpeg.

```
$ git clone https://github.com/libsixel/libsixel
$ cd libsixel
$ ./configure && make install
$ cd ..
$ git clone https://github.com/Albert-S-Briscoe/FFmpeg-SIXEL
$ cd FFmpeg-SIXEL
$ ./configure --enable-libsixel
$ make install
```

If you really don't want to `make install` a random fork of ffmpeg (which is an important dependency to a lot of programs), you can use this awful one-liner to generate another awful one-liner that will let you test ffmpeg-SIXEL. Just replace the `ffmpeg` command with the output. There's probably a better way, but here it is.
```
$ env a="$PWD" bash -c 'echo "env LD_LIBRARY_PATH=\"$a/libavfilter:$a/libavdevice:$a/libswresample:$a/libavcodec:$a/libswscale:$a/libpostproc:$a/libavutil:$a/libavformat:$LD_LIBRARY_PATH\" $a/ffmpeg"'
```

## Examples
Play a video file:
```
$ ffmpeg -i videofilename -f sixel -pix_fmt rgb24 -s 480x270 -
```

If you want audio (and you use linux with pulseaudio), you can use this:
```
$ ffmpeg -i filename -f sixel -pix_fmt rgb24 -s 320x240 - -f alsa -ac 2 pulse
```

Play a YouTube video:
```
$ youtube-dl https://www.youtube.com/watch?v=dQw4w9WgXcQ -o tempvideo
$ ffmpeg -i tempvideo.mkv -f sixel -pix_fmt rgb24 -s 320x240 - -f alsa -ac 2 pulse
```
