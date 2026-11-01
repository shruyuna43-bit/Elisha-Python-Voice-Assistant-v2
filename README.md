# Elisha Python Voice Assistant

A small beginner-friendly assistant that listens through your microphone,
prints recognized words, and prints and speaks its replies.

## What you need

- Python 3.10 or later; Python 3.12 or 3.13 is a practical starting choice.
- A working microphone, speakers or headphones, and microphone permission.
- Internet for Google speech recognition and opening websites.

Speech recognition sends captured audio to Google's recognition service.
Elisha does not save recordings or transcripts to files. Text-to-speech runs
locally. No API key, password, or account setup is required. Recognition service
availability is outside this project's control. Listening begins only when you
run the program; it is not a background or wake-word service.

## Windows setup

1. Download Python from https://www.python.org/downloads/ and enable **Add Python to PATH**.
2. Extract this ZIP. Open a terminal in the extracted `elisha` folder containing `elisha.py`.
3. Create an isolated environment and install the packages:

```powershell
py -m venv .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe elisha.py
```

These commands do not require activating the environment or changing PowerShell policy.

## macOS / Linux setup

On macOS, install PortAudio first with Homebrew: `brew install portaudio`.
On Debian/Ubuntu, install system audio dependencies first:

```bash
sudo apt-get install python3-venv python3-dev portaudio19-dev espeak-ng libespeak1
```

Then, from the extracted project folder:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install --upgrade pip
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python elisha.py
```

Other Linux distributions use different package names. For dependency-specific
setup, see the official [SpeechRecognition](https://pypi.org/project/SpeechRecognition/)
and [pyttsx3](https://pypi.org/project/pyttsx3/) instructions.

## Commands

| Say or type | Result |
| --- | --- |
| hello / hi / hey | Greeting |
| good morning / good afternoon / good evening | Greeting |
| open YouTube | Open YouTube in your default browser |
| open Google | Open Google in your default browser |
| help / what can you do | List commands |
| exit / quit / stop / goodbye / bye | Close Elisha |

You may prefix a command with “Elisha”, for example “Elisha, open Google”.
Speak one command at a time after **Listening...**, then pause. Remain quiet
during the initial one-second calibration. Press **Ctrl+C** to stop at any time.
There is a seven-second limit per spoken phrase. Website commands open only
the two fixed HTTPS addresses; there is no arbitrary command execution.

## Try without a microphone

Use the environment's Python executable from the setup steps in place of `python` below:

```bash
python elisha.py --text
python elisha.py --text --mute
```

The second command uses only Python's standard library: no packages, microphone,
or speech service are needed. Type `hello`, `help`, then `exit` to check the flow.
Website commands still need a browser. `--mute` also works in microphone mode.

## Troubleshooting

- **No microphone:** allow desktop apps microphone access in your operating system,
  connect a microphone, and select it as the default input device. Close apps
  monopolizing the microphone and restart Elisha.
- **PyAudio install fails:** update pip first. On macOS/Linux install the system
  prerequisites above. If using a very new Python version without compatible
  packages, create the environment with Python 3.12 or 3.13 instead.
- **Wrong microphone:** pass `--device-index N`, replacing `N` with the input device's
  index. List devices with your environment's Python:
  `python -c "import speech_recognition as sr; print(list(enumerate(sr.Microphone.list_microphone_names())))"`.
- **No spoken reply:** check output volume and your default speakers. The program
  prints a diagnostic and continues with text if speech initialization fails.
  On Linux, check that eSpeak is installed.
- **Recognition fails:** check internet access, reduce background noise, and speak
  clearly in English. Restart after a recognition-service error, or use `--text`.
- **No browser opens:** the reply shows the website address so you can open it yourself.

## Files and customization

- `elisha.py`: assistant, command handler, and optional keyboard mode.
- `requirements.txt`: voice package dependencies (including PyAudio via the audio extra).
- `.gitignore`: excludes local environments, caches, and common secret files.
- `LICENSE`: MIT license. Update the copyright holder when adopting the project.

To add a command, edit `handle_command()`. Keep actions explicit and avoid executing
recognized speech as shell commands or Python code. Dependency versions are bounded
to major releases; this beginner project does not include a dependency lockfile.

## Verification limits

Command routing and failure handling can be checked with simulated dependencies.
Actual microphone capture, Google recognition, and speaker output require a local
run with installed dependencies and audio hardware.
