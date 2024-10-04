### File: `ai.py`

1. **`load_prompt`**
   - **Description:** Loads the AI system prompt from a specified file and updates the global `system_prompt` variable. Logs success or failure messages.

2. **`save_conversation_to_json`**
   - **Description:** Saves the conversation history to a JSON file if the setting to save chat history is enabled. Logs the result of the operation.

3. **`load_conversation_from_json`**
   - **Description:** Loads conversation history from a JSON file into the `conversation_history` variable. Initializes the variable to an empty list if the file doesn’t exist.

4. **`speak`**
   - **Description:** Converts text to speech using the OpenAI API, saves the audio response to a file, and plays it back using the audio manager. It handles playback in a separate thread and logs relevant information.

5. **`listen`**
   - **Description:** Captures audio input from the microphone, amplifies it, and detects silence to determine when to stop recording. It saves the recorded audio to a file and sends it to the Whisper API for transcription, returning the transcribed text.

6. **`save_conversation`**
   - **Description:** Moves recorded audio and response files to a specified conversation folder, logging the success or failure of each operation.

7. **`hotword_detection_thread`**
   - **Description:** Listens for a hotword using Porcupine, manages LED animations, and handles user input once the hotword is detected. It processes the input, interacts with the OpenAI API for a response, and manages TTS and audio playback.


### File: `commands.py`

1. **`check_reminder_enabled`**
   - **Description:** Checks if the reminder functionality is enabled in the settings. Logs a message if it is not.

2. **`check_alarm_enabled`**
   - **Description:** Checks if the alarm functionality is enabled in the settings. Logs a message if it is not.

3. **`change_light_state`**
   - **Description:** Sends a request to change the state (on/off) of a smart light device based on its ID and logs the action.

4. **`set_reminder`**
   - **Description:** Sets a reminder for a specific timestamp and message. Validates the timestamp format and logs the action.

5. **`set_alarmclock`**
   - **Description:** Sets an alarm clock for specified days and time. Converts days to a human-readable format and logs the alarm setting.

6. **`check_for_command`**
   - **Description:** Processes a JSON response to identify and execute commands such as changing light states, setting reminders, changing LED colors, setting alarms, adjusting volume, or playing a song. Returns a dictionary containing a message to be spoken.


### File: `led_controller.py`

1. **`check_led_enabled()`**
   - Checks if LED functionality is enabled in the settings. Logs an event if not enabled.

4. **`start_rainbow_thread()`**
   - Starts the rainbow effect in a new thread, stopping any current animation first.

6. **`start_breathing(duration, color=Color(255, 0, 0), minbrightness=70, maxbrightness=255)`**
   - Starts the breathing animation in a new thread.

7. **`stop_animation()`**
   - Stops the current animation and turns off all LEDs.

8. **`stop_current_animation()`**
   - Stops the current animation without turning off the LEDs.

9. **`turn_off_leds_instantly()`**
   - Turns off all LEDs immediately.

10. **`turn_off_leds_moving(wait)`**
    - Turns off the LEDs one by one with a delay for a moving effect.

11. **`set_strip_color(color, brightness)`**
    - Sets the color and brightness of the entire LED strip.

13. **`start_linearStart(color=Color(255, 0, 0))`**
    - Starts the linear start effect in a new thread.

15. **`start_sparkle(color=Color(255, 255, 255), wait=0.1)`**
    - Starts the sparkle effect in a new thread.

17. **`start_color_wipe(color=Color(255, 0, 0), wait=0.05)`**
    - Starts the color wipe effect in a new thread.

19. **`start_fade_to_color(color=Color(0, 0, 255), duration=2)`**
    - Starts the fade to color effect in a new thread.

20. **`fire_animation(wait)`**
    - Simulates a flickering fire effect on the LED strip.

22. **`start_aurora_borealis(strip, speed=0.05, intensity=0.8)`**
    - Starts the aurora borealis effect in a new thread.

24. **`start_matrix_rain(strip, color=Color(0, 255, 0), drop_speed=0.2, fade_speed=0.05)`**
    - Starts the Matrix rain effect in a new thread.

25. **`handle_client_data(client_socket)`**
    - Handles incoming client commands for controlling the LED animations.

26. **`if __name__ == "__main__":`**
    - Main loop that sets up the server socket to listen for client connections and handle them in threads.

### File: LedAnimation.py

1. **set_strip_color(color, brightness)**  
   Sends a command to set the LED strip to a specific color and brightness.

2. **start_rainbow_thread()**  
   Initiates a command for a rainbow animation on the LED strip.

3. **start_breathing(color, duration, minbright, maxbright)**  
   Sends a command to create a breathing light effect with specified color, duration, and brightness levels.

4. **start_linearStart(color)**  
   Sends a command to start a linear animation with the specified color.

5. **sparkle(color)**  
   Sends a command to create a sparkle effect with the specified color.

6. **fadetocolor(color, duration)**  
   Sends a command to fade the LED strip to a specified color over a defined duration.

7. **colorwipe(color)**  
   Sends a command to perform a color wipe animation with the specified color.

8. **start_fire()**  
   Initiates a command to create a fire effect on the LED strip.

9. **turn_off_leds_instantly()**  
   Sends a command to instantly turn off all LEDs on the strip.

10. **turn_off_leds_moving(wait)**  
   Sends a command to turn off LEDs with a moving effect, waiting for a specified time.

11. **start_meteor_shower(color, meteor_size, meteor_trail, speed)**  
   Sends a command to start a meteor shower effect with the specified parameters.

12. **start_aurora_borealis(speed, intensity)**  
   Sends a command to create an aurora borealis effect with specified speed and intensity.

13. **start_matrix_rain(color, drop_speed, fade_speed)**  
   Sends a command to initiate a matrix rain effect with specified color, drop speed, and fade speed.

14. **send_command_batch()**  
   Continuously sends commands from the command queue to the LED server at defined intervals.

15. **send_command(command)**  
   Adds a command to the queue for processing and updates the last command time.

### File: SettingsStorage.py

1. **__init__(self, filename: str)**  
   Initializes the `StoreData` class with a specified filename and loads existing data from that file.

2. **_load_data(self) -> None**  
   Loads data from the specified JSON file into a dictionary. If the file does not exist, it initializes an empty dictionary.

3. **_save_data(self) -> None**  
   Saves the current data dictionary to the specified JSON file.

4. **save_data(self, key: str, value: Any, context: str, overwrite: bool = False) -> None**  
   Saves a key-value pair under a specified context, with an option to overwrite existing values.

5. **read_data(self, context: str, key: str = None) -> Any**  
   Reads data from a specific key under a given context. If no key is specified, it returns the entire context.

6. **delete_data(self, key: str, context: str) -> None**  
   Deletes a specific key from a given context in the data dictionary.

7. **key_exists(self, key: str, context: str) -> bool**  
   Checks if a specific key exists within a given context in the data dictionary.

### File: AlarmReminder.py

1. **check_reminder_enabled()**  
   Checks if the reminder functionality is enabled in the settings and logs a message if it is not.

2. **check_alarm_enabled()**  
   Checks if the alarm functionality is enabled in the settings and logs a message if it is not.

3. **parse_timestamp(timestamp)**  
   Parses a timestamp string into a `datetime` object. It supports two formats and logs an error if both fail.

4. **load_reminders()**  
   Loads reminders from a specified file, returning them as a dictionary. If the file does not exist, returns an empty dictionary.

5. **save_reminders(reminders)**  
   Saves the provided reminders dictionary to the specified file in JSON format.

6. **load_alarms()**  
   Loads alarms from a specified file, returning them as a dictionary. If the file does not exist, returns an empty dictionary.

7. **save_alarms(alarms)**  
   Saves the provided alarms dictionary to the specified file in JSON format.

8. **add_reminder(timestamp, message)**  
   Adds a reminder with a specific timestamp and message. If reminders are enabled, it schedules the reminder using a timer.

9. **trigger_reminder(reminder_id, message)**  
   Triggers a reminder by logging the message and removing it from the reminders list.

10. **add_alarm(days, time_str)**  
    Adds an alarm with specified days and time. If alarms are enabled, it schedules the alarm using a timer.

11. **find_next_alarm_time(current_time, alarm_time, days_list)**  
    Determines the next occurrence of the alarm based on the current time, alarm time, and specified days.

12. **trigger_alarm()**  
    Triggers the alarm, logging that the alarm is ringing. Additional actions can be added here.

13. **check_and_schedule_reminders()**  
    Checks all reminders and triggers any that are due. It also schedules upcoming reminders.

14. **check_and_schedule_alarms()**  
    Checks all alarms and schedules those that are due to trigger based on the current time.

15. **main_loop()**  
    The main loop that continuously checks reminders and alarms at defined intervals until a stop event is set.

### File: LogManager.py

1. **`__call__(self, log_type, message)`**  
   This method allows the `LogManager` instance to be called as a function. It logs messages with color coding based on the log type, including the filename and line number of the caller. It retrieves the log type's corresponding color and label, formats the message, and prints it.

### Class Attributes:
- **`LOG_TYPES`**  
  A dictionary mapping log levels to their corresponding color codes and labels. It defines four log types: ERROR, WARNING, INFO, and DEBUG, each with a specific color.

### Initialization:
- **`init(autoreset=True)`**  
  Initializes the `colorama` library to automatically reset the color settings after each print, ensuring subsequent prints are not affected.


### File: AudioManager.py

1. **`__init__(self)`**  
   Initializes the audio manager by setting up the Pygame mixer for audio playback and defining a variable to track the currently playing audio file.

2. **`play_audio(self, file_path)`**  
   Plays an audio file specified by `file_path`. It first stops any currently playing audio, checks if the file exists, and then loads and plays the audio. A separate thread is started to monitor the playback status.

3. **`stop_audio(self)`**  
   Stops any audio that is currently playing. It checks if audio is busy and stops it if so, and then resets the current audio file reference.

4. **`_monitor_audio_playback(self)`**  
   Runs in a separate thread and continuously checks if the audio is still playing. Once playback is complete, it logs a message indicating that playback has finished.

5. **`change_volume(self, volume: float)`**  
   Adjusts the volume of the currently playing audio, where `volume` is expected to be a float between 0.0 (mute) and 1.0 (maximum volume).

6. **`get_volume(self)`**  
   Returns the current volume level of the audio playback.

### Global Instance:
- **`audio_manager`**  
  A global instance of the `AudioManager` class, allowing easy access to its methods throughout the application.


### File: MusicManager.py

1. **`check_music_enabled`**
   - **Description:** Checks if music functionality is enabled in the settings and logs a message if it is not.

2. **`calculate_sha256_hash`**
   - **Description:** Calculates the SHA-256 hash of a given text and logs any errors that occur during the process.

3. **`search_and_download_as_mp3`**
   - **Description:** Searches for a specified query on YouTube, downloads the first result as an MP3 file, and renames it using its SHA-256 hash. It also handles potential errors during the download process.

4. **`play_audio`**
   - **Description:** Plays an audio file using the AudioManager, checking if the file exists before attempting to play it, and logs any errors that occur.
