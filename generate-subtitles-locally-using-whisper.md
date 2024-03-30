---
description: >-
  Easy script to take in an audio or video file and generate srt files using
  whisper cpp
---

# Generate Subtitles locally using whisper



```bash
#!/bin/bash
# Single script to convert a file to wav and have it be transcribed by whisper

# Check if a filename is provided as an argument
if [ -z "$1" ]; then
  echo "Usage: $0 <filename>"
  exit 1
fi

fileName="$1"

# If the file is not already a WAV file, convert it
if [[ $(file -b --mime-type "$fileName") != audio/wav ]]; then
  echo "Converting to WAV file"
  # Modify the conversion command as needed
  ffmpeg -i "$fileName" -acodec pcm_s16le -ar 16000 "$(basename "$fileName" .${fileName##*.}).wav"
  convertedFileName="$(basename "$fileName" .${fileName##*.}).wav"
else
  convertedFileName="$fileName"
fi

if [ ! -f "$convertedFileName" ]; then
  echo "Error: Conversion failed."
  exit 1
fi

# Transcribe the WAV file
echo -e "\n\n\nTranscribing..\n\n\n"
~/code/whisper.cpp/main -osrt -m /Users/aldrinjenson/code/whisper.cpp/models/ggml-medium.en.bin "$convertedFileName" && echo "Transcribed successfully" || echo "Error in transcribing!"

```

### Next steps:

Add the above script to path&#x20;



P.S. Do remember to change the path of whsiper.cpp to the correct location in your computer.
