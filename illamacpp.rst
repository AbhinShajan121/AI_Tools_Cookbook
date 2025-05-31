.. _llamacpp-guide:

Efficient LLMs with llama-cpp-python
====================================

**Purpose**: Run quantized language models with optimized CPU/GPU performance.

Prerequisites
-------------
1. Install with hardware acceleration:
   .. code-block:: bash

      # Basic install (CPU only)
      pip install llama-cpp-python

      # With CUDA (NVIDIA GPU)
      CMAKE_ARGS="-DLLAMA_CUBLAS=on" pip install llama-cpp-python

      # With Metal (Apple M-series)
      CMAKE_ARGS="-DLLAMA_METAL=on" pip install llama-cpp-python

2. Download a quantized model:
   .. code-block:: bash

      wget https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGUF/resolve/main/llama-2-7b-chat.Q4_K_M.gguf

Step-by-Step Implementation
---------------------------

Step 1: Basic Model Loading
~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from llama_cpp import Llama

   # Initialize model
   llm = Llama(
       model_path="llama-2-7b-chat.Q4_K_M.gguf",
       n_ctx=2048,  # Context window size
       n_threads=8  # CPU threads
   )

   # Simple completion
   output = llm.create_completion(
       prompt="Explain AI in simple terms",
       max_tokens=256
   )
   print(output['choices'][0]['text'])

Step 2: Chat Interface
~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   # Chat completion
   response = llm.create_chat_completion(
       messages=[
           {"role": "system", "content": "You're a helpful assistant"},
           {"role": "user", "content": "What's the capital of France?"}
       ],
       temperature=0.7
   )
   print(response['choices'][0]['message']['content'])

Step 3: Advanced Features
~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   # Streaming output
   stream = llm.create_completion(
       prompt="Tell me a story about",
       stream=True,
       max_tokens=500
   )

   for output in stream:
       print(output['choices'][0]['text'], end='', flush=True)

   # Embedding generation
   embedding = llm.create_embedding("Text to embed")
   print(f"Embedding size: {len(embedding['data'][0]['embedding'])}")

Troubleshooting
---------------
- **Slow performance**:
  1. Use smaller quantizations (Q4 instead of Q8)
  2. Enable GPU acceleration:
     .. code-block:: python

        Llama(model_path="...", n_gpu_layers=40)  # All layers to GPU

- **Memory errors**:
  1. Reduce context size (`n_ctx`)
  2. Use CPU offloading:
     .. code-block:: python

        Llama(model_path="...", n_gpu_layers=20)  # Partial offload

- **Model not loading**:
  1. Verify GGUF file integrity
  2. Check architecture compatibility

Advanced Usage
--------------
- **Custom sampling**:
  .. code-block:: python

     output = llm.create_completion(
         prompt="Continue the story:",
         top_k=40,
         top_p=0.95,
         repeat_penalty=1.1
     )

- **Model quantization**:
  Convert models using:
  .. code-block:: bash

     python -m llama_cpp.server --quantize model.bin --qtype q4_k

- **API server**:
  .. code-block:: bash

     python -m llama_cpp.server --model llama-2-7b-chat.Q4_K_M.gguf

Further Resources
-----------------
.. seealso::
   - `llama-cpp-python Docs <https://llama-cpp-python.readthedocs.io>`_
   - `GGML Model Hub <https://huggingface.co/models?search=gguf>`_
   - `Quantization Guide <https://github.com/ggerganov/llama.cpp#quantization>`_