# Multi-Option Video Converter for Premiere Pro - PremiereFlux

A PowerShell script designed to convert and reformat video files to be compatible with Adobe Premiere Pro. The script supports batch processing, video and audio stream handling, hardware-accelerated encoding (H.264 NVENC), and various codec options.

## Key Features

- **Efficient Metadata Handling**:  
  Extracts metadata using `ffprobe` once per file, avoiding redundant calls and improving processing efficiency.
  
- **Video Editing-Friendly Check**:  
  Checks if the video is already in an editing-friendly format (e.g., ProRes, DNxHR, H.264). If so, it can either copy the video stream (without re-encoding) or re-encode it based on the user's choice.

- **Audio Stream Handling**:
  - Downmixes multi-channel audio (e.g., 5.1, 7.1) to stereo if desired.
  - Converts non-PCM audio formats (e.g., AAC, DTS) to uncompressed **PCM (16-bit, 48 kHz)**.
  
- **Batch Output Naming**:
  - User selects a batch output filename template, and the script automatically appends a sequential number to avoid overwriting output files.
  
- **Video Codec Options**:
  - **H.264 (NVENC)**: GPU-accelerated encoding for fast processing (falls back to CPU if CUDA is unavailable).
  - **H.264 (CPU)**: Software-based encoding for general compatibility.
  - **ProRes (CPU)**: High-quality, editing-friendly codec (large file sizes).
  - **DNxHR (CPU)**: Another editing-friendly codec, with various quality options (also CPU-based).

- **Progressive Video Check and Deinterlacing**:
  - Checks if the video is interlaced and applies the **yadif deinterlacing filter** for smooth playback in Premiere Pro.

- **Remuxing Support**:
  - If the video is in a compatible container (e.g., `.mp4` for H.264) but requires a different audio codec or container, the script can remux the video without re-encoding the video stream.

- **Error Handling**:
  - Provides error handling for both `ffprobe` and `ffmpeg`. Logs errors and continues with remaining files.
  - Logs each `ffmpeg` command executed in `BatchConvertLog.txt` for debugging and reference.

## Workflow

1. **User Input**:
   - Choose how to handle editing-friendly video (always re-encode, always copy, or ask per file).
   - Select whether to downmix multi-channel audio to stereo.
   - Choose video encoding format (H.264 NVENC, H.264 CPU, ProRes, DNxHR).
   
2. **File Selection**:
   - Use a `OpenFileDialog` to select one or more files to process.
   
3. **Output Filename Template**:
   - Choose a template for output filenames via a `SaveFileDialog`. For example, if the template is `C:\Output\acatmovie.mp4`, and you select 3 files, the outputs will be `C:\Output\acatmovie_1.mp4`, `C:\Output\acatmovie_2.mp4`, and so on.
   
4. **Processing**:
   - The script processes the video and audio streams based on user settings (encoding, downmix, etc.), renames audio tracks, and ensures the output is compatible with Premiere Pro.

5. **Log File**:
   - After processing, a log file (`BatchConvertLog.txt`) is created with the `ffmpeg` commands executed. This log is saved in the script's directory.

6. **Completion**:
   - The script outputs a message with the number of files processed and the location of the log file.

## Requirements

- **ffmpeg** and **ffprobe** must be installed and added to your system's PATH.
- **H.264 NVENC encoding** requires an **NVIDIA GPU with NVENC support** (the script falls back to CPU encoding if CUDA is unavailable).
- **Windows PowerShell** for script execution.

## How to Use

1. Clone this repository or download the script to your local machine.
2. Ensure that `ffmpeg` and `ffprobe` are installed and accessible via your PATH.
3. Open PowerShell and run the script (`MultiOptionVideoConverter.ps1`).
4. Follow the prompts to select your input files, desired output filename template, and video/audio settings.
5. The script will process the files, and you can track the progress in the `BatchConvertLog.txt` log file.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgements

- This script uses `ffmpeg` and `ffprobe` for video/audio processing and metadata extraction.
- Developed for batch video processing and conversion to optimize compatibility with Premiere Pro.

