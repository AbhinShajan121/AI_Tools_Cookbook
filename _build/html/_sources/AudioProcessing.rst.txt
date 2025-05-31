.. _sounddevice-guide:

Real-Time Audio Processing with sounddevice
===========================================

**Purpose**: Capture, process, and visualize audio streams for AI applications.

Prerequisites
-------------
1. Install required packages:
   .. code-block:: bash

      pip install sounddevice soundfile numpy matplotlib

2. Verify microphone access:
   .. code-block:: python

      import sounddevice as sd
      print(sd.query_devices())  # List available devices

Step-by-Step Implementation
---------------------------

Step 1: Basic Audio Capture
~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import sounddevice as sd
   import soundfile as sf

   def record_audio(duration=5, sample_rate=44100, filename="output.wav"):
       """Record audio to WAV file."""
       print(f"Recording {duration} seconds...")
       audio = sd.rec(int(duration * sample_rate),
                     samplerate=sample_rate,
                     channels=1,
                     dtype='float32')
       sd.wait()  # Block until recording completes
       sf.write(filename, audio, sample_rate)
       return audio

Step 2: Real-Time Stream Processing
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import numpy as np

   def process_audio(indata, frames, time, status):
       """Callback function for live processing."""
       volume = np.linalg.norm(indata) * 10  # Simple volume meter
       print(f"|{'#' * int(volume)}|")

   def live_monitor(duration=10, sample_rate=16000):
       """Monitor audio with real-time feedback."""
       with sd.InputStream(callback=process_audio,
                         channels=1,
                         samplerate=sample_rate):
           sd.sleep(duration * 1000)

Step 3: Audio Visualization
~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import matplotlib.pyplot as plt
   from matplotlib.animation import FuncAnimation

   def visualize_audio():
       """Live spectrogram visualization."""
       fig, ax = plt.subplots()
       line, = ax.plot(np.zeros(44100))
       
       def update_plot(frame):
           audio = sd.rec(44100, samplerate=44100, channels=1)
           sd.wait()
           line.set_ydata(audio[:, 0])
           return line,
       
       ani = FuncAnimation(fig, update_plot, blit=True)
       plt.show()

Troubleshooting
---------------
- **No devices found**:
  1. Check system audio settings
  2. Try specifying device explicitly:
    .. code-block:: python

        sd.default.device = 1  # Use device index from query_devices()

- **Latency issues**:
  - Reduce buffer size:
    .. code-block:: python

       sd.InputStream(blocksize=1024)  # Default is 2048

- **Clipping/distortion**:
  - Normalize audio:
    .. code-block:: python

       audio /= np.max(np.abs(audio))

Advanced Usage
--------------
- **Multi-channel processing**:
  .. code-block:: python

     # Record stereo audio
     sd.rec(..., channels=2)
     # Process left/right channels separately
     left_channel = audio[:, 0]
     right_channel = audio[:, 1]

- **Audio effects pipeline**:
  .. code-block:: python

     def audio_callback(indata, outdata, frames, time, status):
         # Simple echo effect
         outdata[:] = indata + (0.5 * outdata[-frames:])

     with sd.Stream(callback=audio_callback):
         sd.sleep(10000)

Further Resources
-----------------
.. seealso::
   - `sounddevice Documentation <https://python-sounddevice.readthedocs.io>`_
   - `NumPy Audio Processing <https://numpy.org/doc/stable/reference/routines.fft.html>`_
   - `Real-Time Spectrograms <https://matplotlib.org/stable/gallery/animation/spectrogram_demo.html>`_