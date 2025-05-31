.. _speech-recognition-guide:

Speech Recognition with Python
==============================

**Purpose**: Convert spoken audio to text using Python.

Prerequisites
-------------
1. Install required packages:
   .. code-block:: bash

      pip install SpeechRecognition sounddevice soundfile

2. Ensure you have a working microphone

Step-by-Step Implementation
---------------------------

Step 1: Record Audio
~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import sounddevice as sd
   import soundfile as sf

   def record_audio(duration=5, filename="output.wav"):
       """Record audio from default microphone."""
       fs = 44100  # Sample rate
       print(f"Recording for {duration} seconds...")
       recording = sd.rec(int(duration * fs), samplerate=fs, channels=1)
       sd.wait()  # Wait until recording finishes
       sf.write(filename, recording, fs)
       return filename

Step 2: Transcribe Audio
~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import speech_recognition as sr

   def transcribe_audio(audio_file):
       """Convert speech to text using Google's API."""
       r = sr.Recognizer()
       with sr.AudioFile(audio_file) as source:
           audio = r.record(source)
       
       try:
           text = r.recognize_google(audio)
           print(f"Transcription: {text}")
           return text
       except sr.UnknownValueError:
           print("Could not understand audio")
       except sr.RequestError as e:
           print(f"API error: {e}")

Step 3: Run the Pipeline
~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   if __name__ == "__main__":
       audio_file = record_audio(duration=7)
       transcription = transcribe_audio(audio_file)

Troubleshooting
---------------
- **Microphone not working**:
  1. Check system audio settings
  2. Verify device index with ``sd.query_devices()``
- **Poor transcription quality**:
  - Speak clearly near the microphone
  - Reduce background noise
  - Try ``r.adjust_for_ambient_noise(source)``

Advanced Usage
--------------
- For offline recognition (requires Whisper):
  .. code-block:: python

     text = r.recognize_whisper(audio, model="base")

Further Resources
-----------------
.. seealso::
   - `SpeechRecognition Docs <https://pypi.org/project/SpeechRecognition/>`_
   - `SoundDevice API Reference <https://python-sounddevice.readthedocs.io>`_