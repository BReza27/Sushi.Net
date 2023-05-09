## v0.9.2 Beta

This work is based an active development of  on [Sushi](https://github.com/tp7/Sushi](https://github.com/maxpiva/Sushi.Net) and the command syntax is pretty similar
Currently not a whole lot has changed but there is several features that will be added
Reading the orginal original Sushi [Wiki](https://github.com/tp7/Sushi/wiki) will be helpful in knowing what commands change how this works

## Additionaly Sushi.Net supports:
* Automatic Audio Shifting from from different audio sources with same content.
* Automatic Audio Shifting from different audio languages.
* Multiple Subtitles
* Support Resizing the subtitles (ass,ssa) to the destination video


## New features and changes
updated to use .NET 7 
This now allows for a third audio file to be used that will sync using the trims made with the first two
  This is particularly useful when syncing one bluray dubbed audio to a different BD as you can now use the same language for the calculation
  This is meant to be used for anime sources where you can make the trims based on two japanese tracks and apply to the dubbed audio
  

## Notice

As of now I haven't been able to succesfully compile the master this was forked from without it erroring when trying to load the audio file
It also does not currently work with the latest ffmpeg due to changes in parameter values


## To do list

* Impliment differnt audio filters to potentially provide better sync calculations
  This inclued filters such as FFT, lowpass/highpass filtering
  Using ffpmeg to split the frequencies into different bands and using those for creating its chunks rather than silences
  Allow for calcualtions without downsampling
  Better parameters sweet spot, and better vocal filtering.
  

## Downloads

Currently only windows is compiled. 


## Build & Third Party Usage

**Sushi.Net** uses .NET 7 and is a wrapper around **Sushi.Net.Library**.

Binaries builds are published with .NET 7 self-contained and trimmed, any native libraries (Open CV) are included in the build.

**Sushi.Net.Library** can be used in other programs, and it will be uploaded in nuget.org when **Sushi.Net** becomes final.


## How Audio Shifting Works.

Basically, **Sushi.net** will normalize the destination audio stream, then find the silences in the stream, and mark the destination stream into chunks. After that, it will apply a band filter to attenuate the vocals and load both streams into memory. Those chunks will be matched against the source stream. Then the source stream will be reconstructed matching the destination stream chunks positions and saved.

Of course, it can also shift subtitles in the same way original [Sushi](https://github.com/tp7/Sushi) does, and it supports multiple external subtitles as input (space separated) or it can get the subtitles from the --src stream if its a container. (mkv, mp4, etc).


## Example Usage

Being english.mkv and japanese.mkv to different releases from the same show/movie.

```sushi.net --type audio --src english.mkv --dst japanese.mkv```

Will match and English audio stream against a Japanese one, the result will a new English stream that is shifted/synced for the Japanese one.


Being english_tv.mkv and english_dvd.mkv diffrent releases from the same show/movie.

```sushi.net --type audio --src english_tv.mkv --dst english_dvd.mkv```

Will match and English audio tv stream against a dvd one, the result will a new English TV stream that is shifted/synced for the DVD one.


Using audio calculations from two sources with the same audio language track and applying to a differnt audio track 

```--type audio --src "USBD Japanese.flac" --dst "JPBD Japanese.flac" --sync "USBD English.flac"```

Will caclulate trims based on the USBD Japanese audio vs the JPBD Japanese audio and apply the shits to the USBD enlish tracks


## Requirements

[FFMpeg](http://www.ffmpeg.org/download.html) For Audio Extraction & Manipulation.


## Optional Requirements

[MKVExtract](http://www.bunkus.org/videotools/mkvtoolnix/downloads.html) for timecode extraction.

SCXvid for keyframe creation. [Windows](https://github.com/soyokaze/SCXvid-standalone/releases) Version. [Linux](https://eyalmazuz.github.io/Linux_Keyframes/) Version. [Mac](https://eyalmazuz.github.io/Linux_Keyframes/) Version. (Follow the Linux Guide replacing apt with [brew](https://brew.sh/))


## Sushi.Net internally uses the following third party libraries.

[CliWrap](https://github.com/Tyrrrz/CliWrap) - Amazing and Simple way to spawn and manage executables.

[NAudio](https://github.com/naudio/NAudio) - It manages all our wave needs.

[OpenCvSharp](https://github.com/shimat/opencvsharp) - Gives us the avenue to use Open CV.

[OpenCV](https://opencv.org/) - Matches the audio streams.

[Thinktecture.Logging.Configuration](https://github.com/PawelGerr/Thinktecture.Logging.Configuration) - Enables us to change the loglevel on the fly.




## TV Sources Tidbits

* If your source is tv based, some tv channels heavily chops endings, beginnings, credits, scene changes, and scenes with silence, so they can put more advertising per show, they can chop up to 1-3 minutes, for an hour show. Because of that, it's recommended you mute the advertising (not removing in it), and increase the window so matching can be better.
