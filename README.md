# h265ize++

[![NPM License](https://img.shields.io/npm/l/h265ize.svg)](https://raw.githubusercontent.com/Fabricio20/h265ize/master/LICENSE)

> h265ize is a fire and forget weapon. A nodejs utility utilizing ffmpeg to encode large quantities of videos with the hevc codec. For more information visit [ayrton.sparling.us](https://ayrton.sparling.us/index.php/ultimate-x265hevc-encoding-script-h265ize/ "Ayrton Sparling").

> h265ize++ is a fork of the main h265 project, with some added magic dust.

If you have any questions or h265ize isn't working for you, feel free to open an issue.

> _h265ize will support [AV1](https://en.wikipedia.org/wiki/AOMedia_Video_1) once encoder support becomes stable & plex supports decoding it._

## Dependencies

- [Node.js](https://nodejs.org/en/) - Required in order to run h265ize/h265ize++.
- [ffmpeg](https://ffmpeg.org/) - Does the video conversion among other things.

### Optional Dependencies

- [mkvtoolnix](https://www.bunkus.org/videotools/mkvtoolnix/) - Used for upconverting subs in MKVs.
- [vobsub2srt](https://github.com/ruediger/VobSub2SRT) - Used for upconverting subs.

## Table of Contents
- [List of Features](#features)
- [Installation](#installation)
- [Updating](#updating)
- [Uninstalling](#uninstalling)
- [Usage](#usage)
- [Arguments](#arguments)
- [Presets](#presets)
- [Examples](#examples)
- [Stats file](#stats-file)
- [Hardware Acceleration](#hardware-acceleration)
- [Creating 10bit & 12bit encodes](#creating-10bit-&-12bit-encodes)


## Features

**On this fork**:
- CUDA Hardware Acceleration (NVIDIA)
- H264 Support
- Default AAC audio codec
- Custom audio/video target codec support

**Base software**:
- Works on Windows, OSX, and Linux
- Batch file processing (can process a whole folder)
- Automatically detects video files (only processes video files found within a folder)
- Detects all audio tracks
- Preserves audio codecs
- Preserves audio track titles
- Detects and preserves all subtitles
- Detects audio language, if audio language is not your native language and native language subtitles are provided, makes those subtitles default
- Automatically upconvert vobsub/dvdsubs to srt subtitles on mkv files
- Detects bit depth and uses appropriate encoder profile (10-bit is common in high quality anime, supports 8-bit, 10-bit, 12-bit)
- Verbose and preview mode
- File overwrite detection (doesn't accidentally write over a file that already exists, other than in preview mode)
- Detects if file is already encoded in x265 and skips it
- Ability to make encoding previews
- Take screenshots of a finished encode
- Faulty encoding detection based on before and after video durations
- Maintains file structure in output folder (So in theory you could just take your 3tb movie folder and throw it into the script and the output folder should look that same but with x265 videos)

## Installation

To install this fork of h265ize, run the following set of commands:

```
git clone git@github.com:Fabricio20/h265ize.git
cd h265ize
npm install --global
```

Alternatively, clone + cd + run `install.bat` or `install.sh`.

## Updating

Simply run `git pull` inside the cloned repository.

## Uninstalling

To uninstall, run:
`npm uninstall h265ize --global`

## Usage

```
./h265ize [--help] [-d <string>] [-q <0-51>] [-m <string>] [-n <string>] [-f <string>{3}] [-g <string>] [-l <integer>] [-o] [-p] [-v] [--bitdepth (8|10|12)] [--accurate-timestamps] [--as-preset <preset>] [--disable-upconvert] [--debug] [--video-bitrate <integer>] [--he-audio] [--force-he-audio] [--downmix-he-audio] [--screenshots] [--delete] [--hardware] [--h264] [--vcodec <codec>] [--acodec <codec>] <file|directory>
````

## Arguments

List of all arguments available in the software, can also be read by using `h265ize --help`.

### General
```
General:
  -d, --destination      Folder where encoded files are output.
      [default: "./h265/"]
  -m, --preset           x265 encoder preset.
      [choices: "ultrafast", "superfast", "veryfast", "faster", "fast", "medium", "slow",  "slower", "veryslow", "placebo"]
      [default: "fast"]
  -n, --native-language  The native language used to select default audio and
                         subtitles. You may use 3 letter or 2 letter ISO 639-2
                         Alpha-3/Alpha-2 codes or the full language name.
      [examples: "eng", "en", "English", "jpn", "ja", "Japanese"]
      [default: ""]
  -f, --output-format    Output container format.
      [choices: "mkv", "mp4", "m4v"]
      [default: "mkv"]
  -q, --quality          Sets the qp quality target       
      [number]
      [default: 19]
  -o, --override         Enable override mode. Allows conversion of videos that
                         are already encoded by the hevc codec.
      [boolean]
      [default: false]
  -p, --preview          Only encode a preview of the video starting at middle
                         of video. See -l/--preview-length for more info.
      [boolean]
      [default: false]
  -v, --verbose          Enables verbose mode. Prints extra information.
      [boolean]
      [default: false]

Options:
  --help     Displays help page.
  --version  Displays version information.
```

### Video
```
Video:
  --as-preset            My personal presets. Descriptions of each preset's use
                         and function can be found on the github page.
      [choices: "anime-high", "anime-medium", "testing-ssim", "none"]
      [default: "none"]
  -x, --extra-options    Extra x265 options. Options can be found on the x265
                         options page.
      [string]
      [default: ""]
  --video-bitrate        Sets the video bitrate, set to 0 to use qp rate control
                         instead of a target bitrate.
      [number]
      [default: 0]
  --accurate-timestamps  Become blu-ray complient and reduce the max keyInt to
                         the average frame rate.
      [boolean]
      [default: false]
  --multi-pass           Enable multiple passes by the encoder. Must be greater
                         than 1.
      [number]
      [default: 0]
  --bitdepth             Forces encoding videos at a specific bitdepth. Set to 0
                         to maintain original bitdepth.
      [number]
      [default: 0]
  --screenshots          Take 6 screenshots at regular intervals throughout the
                         finished encode.             
      [boolean]
      [default: false]
  --scale                Width videos should be scaled to. Videos will always
                         maintain original aspect ratio. [Examples: 720, 480]
      [number]
      [default: false]
```

### Audio

```
Audio:
  --he-audio          Re-encode audio to opus at 40kbps/channel.
      [boolean]
      [default: false]
  --force-he-audio    Convert all audio to HE format, including lossless
                      formats.
      [boolean]
      [default: false]
  --downmix-he-audio  Downmix he-audio opus to Dolby Pro Logic II at 40
                      kbps/channel. Enables he-audio.
      [boolean]
      [default: false]
```

### Advanced

```
Advanced:
  -l, --preview-length  Milliseconds to encode in preview mode. Max is half the
                        length of input video.
      [number]
      [default: 30000]
  --stats               Output a stats file containing stats for each video
                        converted.
      [boolean]
      [default: false]
  --watch               Watches a directory for new video files to be converted.
      [string]
      [default: ""]
  --normalize-level     Level of normalization to be applied. See
                        https://github.com/FallingSnow/h265ize/issues/56 for
                        more info.
      [number]
      [default: 2]
  --debug               Enables debug mode. Prints extra debugging information.
      [boolean]
      [default: false]
  --delete              Delete source after encoding is complete and replaces it
                        with new encode. [DANGER]
      [boolean]
      [default: false]
  --test                Puts h265ize in test mode. No files will be encoded.
      [boolean]
      [default: false]
  --hardware            Enables hardware acceleration (NVIDIA).
      [boolean]
      [default: false]
  --h264                Uses h264 instead of h265.
      [boolean]
      [default: false]
  --vcodec              Use a specific video codec (check ffmpeg for list of supported values).
      [string]
      [default: false]
  --acodec              Use a specific audio codec (check ffmpeg for list of supported values).
      [string]
      [default: false]
```

## Presets

   Preset    | Description
:----------: | :----------------------------------------------------------------------------
   anime-high     | A very good preset for all types of anime. Produces very good quality for a very small size.
   anime-medium     | Same as anime-high but uses debanding to produce better color gradients.
testing-ssim | x265's native preset just in SSIM mode.

## Examples

- `h265ize -v big_buck_bunny_1080p_h264.mov`
- `h265ize -v -d /home -q 25 big_buck_bunny_folder`
- `h265ize -d /home -q 25 --watch videos/folder`

## Stats file

The stats file is located at the current working directory under the name `h265ize.csv`. This must be enabled using the `--stats` flag. The file is composed of several lines. Each line is in the format

`[Finish Encoding Date],[File Path],[Original Size],[Encoded size],[Compression Precent],[Encoding Duration]`

For example:

`08/13 02:46:03 PM, videos/[deanzel] Noir - 08 [BD 1080p Hi10p Dual Audio FLAC][a436a4e8].mkv, 1964MB, 504MB, 25.66%, 2:51:16`

## Hardware Acceleration

It is possible to run h265ize using hardware acceleration, at least on supported nvidia GPUs. To be able to run with hardware acceleration, your copy of ffmpeg must support `nvenc`.

To enable hardware acceleration, simply pass `--hardware` to h265ize. You should see the codec change to `hevc_nvenc` or `h264_nvenc`.

## Creating 10bit & 12bit encodes

To create 10 or 12bit encodes, simply pass the `--bitdepth 10` or `--bitdepth 12` parameters. Make sure you have the correct libraries or ffmpeg static build.
