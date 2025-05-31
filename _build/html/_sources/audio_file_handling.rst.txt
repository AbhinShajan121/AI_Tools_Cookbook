.. _soundfile-guide:

Audio File Handling with soundfile
==================================

**Purpose**: Read, write, and convert audio files for AI applications.

Prerequisites
-------------
1. Install required packages:
   .. code-block:: bash

      pip install soundfile numpy

2. Supported formats:
   - WAV, FLAC, OGG (requires libsndfile)
   - MP3 (requires additional backend like pydub)

Step-by-Step Implementation
---------------------------

Step 1: Basic Audio I/O
~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import soundfile as sf

   # Write audio to file
   def save_audio(data, filename, sample_rate=44100):
       """Save numpy array as audio file."""
       sf.write(filename, data, sample_rate)
       print(f"Saved {filename}")

   # Read audio file
   def load_audio(filename):
       """Load audio file into numpy array."""
       data, sr = sf.read(filename)
       print(f"Loaded {filename} | Shape: {data.shape} | SR: {sr}Hz")
       return data, sr

Step 2: Format Conversion
~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   def convert_audio(input_file, output_file, output_format):
       """Convert between audio formats."""
       data, sr = sf.read(input_file)
       sf.write(output_file, data, sr, format=output_format)
       print(f"Converted {input_file} to {output_format}")

   # Example usage:
   # convert_audio("input.wav", "output.flac", "FLAC")

Step 3: Advanced Processing
~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import numpy as np

   def process_stereo(filename):
       """Process multi-channel audio."""
       data, sr = sf.read(filename, always_2d=True)
       
       # Extract channels
       left = data[:, 0]
       right = data[:, 1]
       
       # Create mono mix
       mono = np.mean(data, axis=1)
       
       # Normalize audio
       mono /= np.max(np.abs(mono))
       return mono, sr

Troubleshooting
---------------
- **Unsupported format error**:
  1. Install libsndfile:
     .. code-block:: bash

        # Linux
        sudo apt-get install libsndfile1

        # Mac
        brew install libsndfile

- **MP3 files not working**:
  Use pydub as backend:
  .. code-block:: python

     from pydub import AudioSegment
     AudioSegment.from_mp3("input.mp3").export("output.wav", format="wav")

- **Large file handling**:
  Process in chunks:
  .. code-block:: python

     with sf.SoundFile("large.wav") as f:
         while True:
             chunk = f.read(1024)
             if len(chunk) == 0:
                 break
             process(chunk)

Advanced Usage
--------------
- **Metadata handling**:
  .. code-block:: python

     with sf.SoundFile("audio.wav") as f:
         print(f"Duration: {len(f)/f.samplerate:.2f}s")
         print(f"Channels: {f.channels}")

- **Batch processing**:
  .. code-block:: python

     import os
     def batch_convert(input_dir, output_format):
         for file in os.listdir(input_dir):
             if file.endswith(".wav"):
                 output_file = f"{os.path.splitext(file)[0]}.{output_format.lower()}"
                 convert_audio(os.path.join(input_dir, file), 
                             os.path.join(input_dir, output_file),
                             output_format)

Further Resources
-----------------
.. seealso::
   - `soundfile Documentation <https://pysoundfile.readthedocs.io>`_
   - `Audio File Formats <https://en.wikipedia.org/wiki/Audio_file_format>`_
   - `Libsndfile Website <http://www.mega-nerd.com/libsndfile/>`_