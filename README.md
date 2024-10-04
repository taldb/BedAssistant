

# BedAssistant
<img src="https://raw.githubusercontent.com/taldb/BedAssistant/8ee4adcef926e802df61b42096b80957aeeb9580/assets/AIGENERATED_LOGO.png" alt="DALLE generated image" width="300" height="300"/>

# disclaimer - this is still in development, by a solo developer expect bugs!

BedAssistant is an audio assistant that’s powered by openai **Services** and designed to make your life a little easier (and a lot more fun!).
it can listen for your commands, engages in real conversations, and responds with a friendly voice.

## Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
~~- [Configuration](#configuration)~~
	- [Settings.json](#settings.json file)
~~- [Audio Settings](#audio-settings)~~
~~- [Voice Settings](#voice-settings)~~
~~- [Advanced](https://github.com/taldb/BedAssistant/blob/main/functionindex.md)~~
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)



## Overview

### How It Works

1. **Listening for the Hotword**:
   - The assistant is always on the lookout for a specific hotword. It uses **Picovoice Porcupine** to keep its ears open without using too much processing power.

2. **Activating the Assistant**:
   - Once it hears the hotword, the assistant immediately stops any music or audio that's playing.
3. **Recording Your Voice**:
   - After it’s activated, the assistant starts recording what you say. It uses **PyAudio** to capture your voice clearly.

4. **Enhancing Audio Quality**:
   - The recorded audio gets processed. it boosts the volume and reduces any background noise to ensure it captures your voice perfectly.

5. **Transcribing Your Words**:
   - Once it has a recording, the audio is sent off to **OpenAI’s Whisper API** for transcription.
   -  it transcribes the recorded audio (quality depends on microphone settings and microphone quality).

6. **Chatting with GPT**:
   -  the assistant sends the transcribed text to **OpenAI’s GPT** to craft a thoughtful response. It remembers the context of your previous chats to keep the conversation flowing naturally.
   
7. **Parsing Command**:
	- the assistant checks if the response from GPT is json (it should be), if it is, it parses the "**ActionType**"  Value, 
	- dive More into [**ActionType Here**](#actiontype) 

8. **Turning Text into Speech**:
   - Once the assistant has Parsed the json and extracted the "**ttsmessage**" field, it converts the text into speech using 	**OpenAI's TTS Service**. 

9. **Playing the Response**:
   - The assistant then plays back its response through speakers.

10. **Ready for the Next Interaction**:
    - After delivering its response, the assistant goes back to listening for the hotword, ready in action whenever you need it!.

## Features
#### AI Interaction
- **Prompt Management**: Load and save AI system prompts for customizable interaction.
- **Conversation History**: Automatically save and load conversation history in JSON format, enabling seamless continuity in interactions.
- **Text-to-Speech (TTS)**: Convert text responses to speech using the OpenAI API, with playback handled efficiently.
- **Voice Input**: Capture and transcribe audio input, enhancing hands-free operation.

#### Task Management
- **Reminders & Alarms**: Set and manage reminders and alarms with support for various configurations. Notifications are triggered based on scheduled times.
- **Command Execution**: Process voice commands for various actions, including reminders, alarms, and smart device control.

#### LED Control
- **LED Animations**: Control LED animations with various effects like rainbow, breathing, sparkle, fire, and more, allowing for dynamic lighting experiences.
- **Instant Control**: Turn LEDs on or off instantly or with graceful transition effects.

#### Audio Management
- **Audio Playback**: Play audio files with volume control and playback monitoring, providing a complete audio experience.
- **Music Integration**: Search and download music from platforms like YouTube and manage music playback effectively.

#### Data Management
- **Settings Storage**: Load, save, and manage settings for various functionalities, ensuring a personalized experience. All features are customizable through the [Settings.json](#Settings.json) file.
- **Logging**: Comprehensive logging of actions and events for debugging and monitoring purposes.

#### Hotword Detection
- **Voice Activation**: Utilize hotword detection to activate the assistant, enabling quick access without manual input.


## Settings.json file
```yaml
{
    "General": {
        "audio_settings": {
            "tts_voice": "alloy",
            "tts_model": "tts-1",
            "ai_model": "gpt-4o-mini",
            "transcription_model": "whisper-1",
            "transcription_language": "en"
        },
        "recording_settings": {
            "_comment_notused": "change this according to your microphone",
            "recording_audio_filename": "recorded_audio.wav",
            "sample_rate": 44100,
            "chunk_size": 1024,
            "silence_threshold": 500,
            "silence_duration": 1.7,
            "amplification_factor": 3.162,
            "channels": 1
        },
        "api_keys": {
            "openai_auth": "YOUR OPENAI TOKEN",
            "pvporcupine_auth": "YOUR PICOVOICE TOKEN"
        },
        "led_settings": {
            "enabled": true,
            "led_count": 252,
            "strip_gpio_pin": 18,
            "strip_brightness": 255,
            "led_frequency_hz": 800000,
            "led_dma_channel": 10,
            "led_invert": false,
            "led_channel": 0
        },
        "other": {
            "save_chat_history": true,
            "alarm_enabled": true,
            "reminder_enabled": true,
            "alarmclock_check_interval": 30
        },
        "music_settings": {
            "enabled": true,
            "ffmpeg_location": "/usr/bin/ffmpeg",
            "format": "bestaudio/best",
            "preferredcodec": "mp3",
            "preferredquality": "192"
        }
    },
    "Files": {
        "premade_tts": {
            "_comment_notused": "those come premade, note they use the alloy voice",
            "1": "premade_tts\\request_proccess_error.mp3",
            "2": "premade_tts\\change_led_color_error.mp3",
            "3": "premade_tts\\deviceerror.mp3",
            "4": "premade_tts\\json_command_process_error.mp3",
            "5": "premade_tts\\play_song_error.mp3",
            "6": "premade_tts\\retrieve_song_error.mp3",
            "7": "premade_tts\\set_alarm_error.mp3",
            "8": "premade_tts\\set_reminder_error.mp3",
            "9": "premade_tts\\unexpected_error.mp3",
            "10": "premade_tts\\RESERVED"
        },
        "ai_tts_response_dir": "tts",
        "_comment_notused": "change this to your hotword file",
        "pvporcupine_hotword_file": "heygpt_rec_rpi.ppn",
        "prompt": "./Data/prompt.txt",
        "chathistory": "./Data/history.json",
        "reminders": "./Data/reminders.json",
        "alarms": "./Data/alarms.json"
    },
    "command_settings": {
        "smarthome_API": {
            "_comment_notused": "my api service i made in C# (still in development)",
            "enabled": true,
            "url": "http://xxxxxxxxx/smart/changedevicestate",
            "websocket_url": "ws://xxxxxx",
            "BearerToken": ""
        }
    }
}
```


## ActionType
The `ActionType` in the provided context specifies the type of command that the virtual assistant needs to generate based on user requests. Each command corresponds to a specific action the assistant can perform, ensuring that the user’s requests are accurately translated into structured JSON format for processing. Here’s a breakdown of the `ActionType` values and their meanings:

1. **`changestate`**: This action controls the state of lights (turning them on or off). It requires a `DeviceID` to identify which light to control and a `State` to specify whether to turn it on (1) or off (0).
 **this is for my api service i made (still in development but will be released soon...)**

3. **`setReminder`**: This action sets a reminder with a specific timestamp and message. The timestamp must be formatted correctly, and if no message is provided, a default message is used.

4. **`setAlarmclock`**: This action schedules an alarm for specified days and a specific time. The days are represented numerically (0 for Sunday, 1 for Monday, etc.).

5. **`playsong`**: This action plays a specified song using YouTube music, and it includes the song's name in the command.

6. **`changevolume`**: This action adjusts the audio volume to a specified level. The volume is represented as a float, where 1 corresponds to 100%.

7. **`changeledcolor`**: This action changes the color of the device's LED lights. It requires RGB values and can include a brightness level. Special cases like "rainbow" colors are also accounted for.

8. **General chat responses**: For casual interactions or general questions, the response will simply contain a friendly message or answer without the need for an `ActionType`.

Each `ActionType` not only directs the assistant on what action to take but also includes a `ttsmessage` field for providing spoken feedback to the user, enhancing the interactivity and user experience.

### Examples:
#### Example 1:
**input**: "Set volume to 50%"
  **output (GPT)**:  ``` {"ActionType":"changevolume","volume":"0.5","ttsmessage":"Changed volume to 50%."}```

#### Example 2:
**input**: "set a reminder today at 11:05 PM"
**output(GPT)**:
```{"ActionType":"setReminder","timestamp":"DD-MM-YY 23:05","Message":"<your message>","ttsmessage":"Reminder set for today at 11:05 PM."}```




### Prerequisites

#### Hardware
- **Raspberry Pi** (or any compatible computer)
- **Microphone**: USB microphone recommended for optimal audio input
- **Speakers**: External speakers for audio output
- **LED Strip**: Optional for enhanced visual feedback

#### Software
- **Python**: Version 3.x (ensure it's installed)
#### **Required Libraries**:
- **colorama**
- **numpy**
- **openai**
- **PyAudio**
- **requests**
-  **Pygame**
 -	**full list in the requirements.txt**

#### optional:
- **RPi.GPIO (for other tasks)**
- **yt-dlp (for music)**
-  **rpi-ws281x (for leds)**

## Installation

#### windows:
```bash 
git clone https://github.com/taldb/BedAssistant.git
cd BedAssistant
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

#### Linux:
``` bash
sudo apt update
sudo apt install git alsa-utils pulseaudio
git clone https://github.com/taldb/BedAssistant.git
cd BedAssistant
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt

```
## Usage
### to run the assistant:
- make sure you changed the settings in your [settings.json](#settings.json file) file
- if you are using led's, you need to run the `LedAnimation_Socket.py` that is located under `"LED/LedAnimation_Socket.py"` with **root privileges (sudo)**, the python script cannot access the raspberry pi gpio pins without root 
(if you are using a raspberry pi)

- Run the `ReminderManager.py` that is located under `AITools/ReminderManager.py` (if you are using reminders/clocks).

- Run the `ai.py` in the root directory (make sure you are in the python environment), **without root (sudo)**

## Contributing

We welcome contributions to this project! If you'd like to help, please follow these guidelines:

1. **Fork the Project**: Create a personal copy of the repository by forking it.
2. **Create a New Branch**: Switch to a new branch for your feature or bug fix.
   ```bash
   git checkout -b feature-branch
   ```
3. **Make Your Changes**: Implement your changes in the code.
4. **Test Your Changes**: Run the existing test suite to ensure everything works correctly. If applicable, add new tests for your changes.
5. **Commit Your Changes**: Save your changes with a meaningful commit message.
   ```bash
   git commit -m "Add new feature"
   ```
6. **Push to the Branch**: Upload your changes to your forked repository.
   ```bash
   git push origin feature-branch
   ```
7. **Open a Pull Request**: Go to the original repository and submit a pull request, describing the changes you've made.

Thank you for your contributions! Your efforts help improve the project for everyone!

## License
This project is licensed under the  Attribution-NonCommercial 4.0 International License - see the [LICENSE](LICENSE) file for details.

## Contact

Provide your contact information or links to your social media profiles for further questions or collaborations.

- dennis/tal - [tald.baror@gmail.con](mailto:tald.baror@gmail.com) - Please not i will **NOT** answer to anyone that will spam me!
- GitHub: [taldb](https://github.com/taldb)

## TODO:
- add more info in this README.md
