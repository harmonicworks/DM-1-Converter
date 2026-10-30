# DM-1 Converter

**Turn Olympus DSS voice recordings into WAV or MP3, right in your browser.**

The Olympus DM-1 (2001) and other dictaphones of that era record in DSS, a compressed dictation format that most software can't open anymore. DM-1 Converter reads those files, detects SP or LP mode by itself, and gives you ordinary audio files. It can also trim and clean them up, transcribe speech to text, and, for sound designers, mangle any audio the way the DM-1 does.

- **[Open DM-1 Converter in your browser](https://harmonicworks.github.io/DM-1-Converter/)**: nothing to install
- Everything runs on your own computer. Your recordings are never uploaded.

> Not affiliated with Olympus / OM Digital Solutions. Tested with one DM-1 so far (SP and LP), so please share how it works with your recorder.

## What you need

- DSS files from the recorder's card (a SmartMedia card reader works), or from the recorder over USB
- A recent desktop browser (Chrome, Edge, Firefox or Safari)
- An internet connection the first time: the audio engine (~32 MB) and the speech model are downloaded once and then cached

## Converting

1. Drop the `.DSS` files, or the whole card folder, onto the page. Other files on the card are skipped.
2. Each recording gets a badge (**SP** or **LP**), its recording date and its length.
3. Pick a recording to see the waveform. Drag the red edges to trim it, and choose **Normalize**, **Low-cut** (removes rumble and hum) and fades if you want.
4. Choose a format:
   - **WAV**, mono 16-bit: native rate (8 or 11 kHz), 22.05 kHz or 44.1 kHz
   - **MP3**, mono: 64 kbps or 128 kbps
5. Press **Save this file**, or **Save all (ZIP)**.

Files are named from the recording's own start time, like `2026-10-30 1924 DM-1 SP.wav`. You can change the naming pattern (`{date} {time} {device} {mode} {orig} {n}`) or give a file its own name.

## Transcribing

The **Transcribe** tab turns speech into text with [Whisper](https://github.com/openai/whisper), running in your browser.

- Choose a model: **Tiny** (fast, rough), **Base** (balanced) or **Small** (most accurate, slower)
- Choose the spoken language (around 60 are available). Optionally translate to English
- Optional timestamps, like `[0:12]`
- Edit the text, then save it as a Markdown file. Transcripts are included in the ZIP

Be realistic about the result: DSS is telephone-quality audio, so mumbling and noise lead to mistakes. SP mode transcribes better than LP.

## Create (sound design)

For musicians and anyone who likes lo-fi sound. These tools work on DSS recordings and on any audio file you drop in (WAV, MP3, AIFF, FLAC…):

- **Varispeed**: tape-style, slower is lower
- **Reverse**
- **DM-1 treatment**: runs the sound through G.723.1 at 8 kHz, the codec the DM-1 uses in LP mode, from 1 up to 20 generations
- **VCVA gate**: like the recorder's voice-activated recording, the quiet parts are cut out

Effects are included when you export. **Preview result** lets you hear everything first.

## About the format

| | SP | LP |
|---|---|---|
| Codec | DSS SP | G.723.1 |
| Sample rate | 11,025 Hz | 8,000 Hz |
| Sound up to | ~5 kHz | ~3.5 kHz |
| Data rate | ~14 kbit/s | ~6 kbit/s |

More detail, including the file header layout, is in [FORMAT.md](FORMAT.md). DS2 files (DSS Pro, from later recorders) are not supported.

## Credits

- Idea, design & testing: **Maurice Hissink** ([harmonicworks](https://github.com/harmonicworks))
- Code: **Ember** (Claude, by Anthropic)

Built on [FFmpeg](https://ffmpeg.org) via [ffmpeg.wasm](https://github.com/ffmpegwasm/ffmpeg.wasm) (decoding and encoding), [Transformers.js](https://github.com/huggingface/transformers.js) with OpenAI's Whisper models (transcription) and [JSZip](https://stuk.github.io/jszip/).

## License

GPL-3.0, see [LICENSE](LICENSE). The FFmpeg build this tool uses is GPL-licensed, so the tool is too: you're free to use, change and share it, as long as changed versions stay open.
