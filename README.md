# 🎙️ Real-Time AI Voice Assistant

A low-latency, real-time conversational AI voice assistant built in Python. This project utilizes local Voice Activity Detection (VAD) to listen naturally, transcribes speech offline using Whisper, streams intelligent responses via Cohere's latest command models, and speaks the response aloud in real-time using Microsoft Edge's neural text-to-speech.

## ✨ Features
* **Natural Listening:** Uses `SpeechRecognition` to automatically detect when you start and stop talking—no push-to-talk button required.
* **Local Offline Transcription:** Uses OpenAI's `Whisper` (Base model) to ensure fast and private speech-to-text.
* **Streaming LLM:** Utilizes Cohere's fast `command-r7b-12-2024` model. Responses are streamed token-by-token to eliminate waiting times.
* **Real-Time Neural Audio:** Uses `RealtimeTTS` with `EdgeEngine` to speak the AI's response simultaneously as it is being generated, bypassing standard Windows audio thread locks.
* **Conversational Formatting:** Custom system prompts strip out markdown, asterisks, and bullet points so the bot sounds human and conversational.

## 🛠️ Tech Stack
* **LLM:** [Cohere](https://cohere.com/) (`command-r7b-12-2024`)
* **STT:** [OpenAI Whisper](https://github.com/openai/whisper)
* **TTS:** [RealtimeTTS](https://github.com/KoljaB/RealtimeTTS) (`EdgeEngine`)
* **Audio Interfacing:** `SpeechRecognition`, `PyAudio`

---

## 🚀 Installation & Setup

### 1. Install Python Dependencies
Ensure you have Python installed, then run the following command to install the required libraries:

```bash
pip install cohere openai-whisper SpeechRecognition pyaudio "RealtimeTTS[edge]"
```

### 2. Install Audio Processing Tools
This project requires a few external system tools to handle audio recording and playback on Windows:

* **FFmpeg (Required for Whisper):**
  * Download FFmpeg and add it to your Windows System PATH, or install it via winget:
    ```cmd
    winget install ffmpeg
    ```
* **MPV (Required for Edge TTS):**
  * Download the `mpv.exe` binary from [SourceForge](https://sourceforge.net/projects/mpv-player-windows/files/).
  * Extract the zip file and place `mpv.exe` **directly inside the same folder** as your Python script.

### 3. API Key Setup
You will need a free API key from Cohere.
1. Sign up at the [Cohere Dashboard](https://dashboard.cohere.com/).
2. Generate an API key.
3. Open the main python file and replace `"YOUR_COHERE_API_KEY"` with your actual key.

---

## 💻 Usage

Run the main Python script:
```bash
python main.py
```
1. Wait a few seconds for the system to load the Whisper model into memory and calibrate to your room's ambient noise.
2. When the console says `--- System Ready! ---` and `Listening (Speak now)...`, simply start talking.
3. The system will automatically detect when you stop speaking, process the audio, and reply verbally.
4. Say "exit", "stop", or "quit" to shut down the assistant safely.

---

## 🔧 Troubleshooting

**Error: `OMP: Error #15: Initializing libiomp5md.dll, but found libiomp5md.dll already initialized.`**
* **Cause:** A conflict between PyTorch and Anaconda's OpenMP libraries on Windows.
* **Fix:** The script automatically handles this using `os.environ["KMP_DUPLICATE_LIB_OK"] = "TRUE"`. Do not remove this line from the top of the file.

**Error: `mpv not found, necessary to stream audio.`**
* **Cause:** The RealtimeTTS EdgeEngine cannot locate the mpv audio player.
* **Fix:** Ensure `mpv.exe` is placed directly in the same directory as your Python script, or added to your Conda Environment's `Scripts` folder.

**Warning: `UserWarning: FP16 is not supported on CPU; using FP32 instead`**
* **Cause:** PyTorch is running Whisper on your CPU instead of an NVIDIA GPU.
* **Fix:** The code handles this gracefully by enforcing `fp16=False` during transcription. If you have an NVIDIA GPU and want faster processing, install the CUDA version of PyTorch and move the whisper model to `.to("cuda")`.

---

## 📝 License
This project is open-source and available under the MIT License.
