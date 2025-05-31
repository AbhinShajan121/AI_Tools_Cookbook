.. _ollama-guide:

Local AI with Ollama
====================

**Purpose**: Run and customize open-weight language models locally.

Prerequisites
-------------
1. Install Ollama:
   .. code-block:: bash

      # Linux/macOS
      curl -fsSL https://ollama.com/install.sh | sh

      # Windows (Powershell)
      winget install ollama

2. Verify installation:
   .. code-block:: bash

      ollama --version

Step-by-Step Implementation
---------------------------

Step 1: Model Management
~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: bash

   # List available models
   ollama list

   # Pull a model
   ollama pull mistral

   # Remove a model
   ollama rm mistral

Step 2: Basic Python Integration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   import ollama

   # Simple chat completion
   response = ollama.chat(
       model='mistral',
       messages=[{
           'role': 'user',
           'content': 'Explain quantum computing in simple terms'
       }]
   )
   print(response['message']['content'])

Step 3: Advanced Features
~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   # Stream responses
   stream = ollama.chat(
       model='llama2',
       messages=[{'role': 'user', 'content': 'Tell me about Paris'}],
       stream=True
   )

   for chunk in stream:
       print(chunk['message']['content'], end='', flush=True)

   # Custom model parameters
   response = ollama.generate(
       model='codellama',
       prompt='def factorial(n):',
       options={
           'temperature': 0.7,
           'num_predict': 128
       }
   )

Troubleshooting
---------------
- **Model not found**:
  1. Verify model name:
     .. code-block:: bash

        ollama list
  2. Pull the model first:
     .. code-block:: bash

        ollama pull model_name

- **Slow performance**:
  1. Use smaller models (e.g., mistral instead of llama2-70b)
  2. Enable GPU acceleration:
     .. code-block:: bash

        OLLAMA_NO_CUDA=0 ollama serve

- **Memory issues**:
  1. Reduce context window:
     .. code-block:: python

        options={'num_ctx': 2048}

Advanced Usage
--------------
- **Custom model creation**:
  1. Create Modelfile:
     .. code-block:: text

        FROM mistral
        SYSTEM """You are a helpful AI assistant specialized in Python programming"""
        TEMPLATE """{{ .System }}

        User: {{ .Prompt }}
        Assistant: {{ .Response }}"""

  2. Build and run:
     .. code-block:: bash

        ollama create my-ai -f Modelfile
        ollama run my-ai

- **API endpoint**:
  .. code-block:: python

     import requests

     response = requests.post(
         'http://localhost:11434/api/generate',
         json={
             'model': 'mistral',
             'prompt': 'Explain blockchain'
         }
     )

Further Resources
-----------------
.. seealso::
   - `Ollama Documentation <https://github.com/ollama/ollama>`_
   - `Model Library <https://ollama.ai/library>`_
   - `Python API Reference <https://github.com/ollama/ollama-python>`_