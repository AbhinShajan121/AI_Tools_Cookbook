.. _gemma-guide:

Google Gemma Models
===================

**Purpose**: Run and fine-tune Google's lightweight open models efficiently.

Prerequisites
-------------
1. Install required packages:
   .. code-block:: bash

      pip install transformers torch sentencepiece

2. Accept license terms:
   - Visit https://huggingface.co/google/gemma-7b
   - Click "Agree and access repository"

Step-by-Step Implementation
---------------------------

Step 1: Basic Inference
~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from transformers import AutoTokenizer, AutoModelForCausalLM

   # Load 2B model (faster for testing)
   tokenizer = AutoTokenizer.from_pretrained("google/gemma-2b-it")
   model = AutoModelForCausalLM.from_pretrained(
       "google/gemma-2b-it",
       device_map="auto",
       torch_dtype="auto"
   )

   # Generate text
   input_text = "Explain attention in transformers"
   inputs = tokenizer(input_text, return_tensors="pt").to("cuda")
   outputs = model.generate(**inputs, max_new_tokens=100)
   print(tokenizer.decode(outputs[0]))

Step 2: Chat Format
~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   # Proper chat formatting
   chat = [
       {"role": "user", "content": "Who invented Python?"}
   ]
   prompt = tokenizer.apply_chat_template(chat, tokenize=False)
   inputs = tokenizer(prompt, return_tensors="pt").to("cuda")
   outputs = model.generate(**inputs, max_new_tokens=100)
   print(tokenizer.decode(outputs[0]))

Step 3: Quantization (4-bit)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from transformers import BitsAndBytesConfig

   quant_config = BitsAndBytesConfig(
       load_in_4bit=True,
       bnb_4bit_compute_dtype=torch.float16
   )

   model = AutoModelForCausalLM.from_pretrained(
       "google/gemma-7b",
       quantization_config=quant_config,
       device_map="auto"
   )

Troubleshooting
---------------
- **License Errors**:
  1. Log in to Hugging Face:
     .. code-block:: bash

        huggingface-cli login
  2. Accept model agreement

- **Memory Issues**:
  1. Use smaller model (2B instead of 7B)
  2. Enable 4-bit quantization
  3. Try CPU offloading:
     .. code-block:: python

        device_map = {
            "transformer.word_embeddings": 0,
            "transformer.final_layer_norm": "cpu",
            "lm_head": "cpu"
        }

- **Slow Performance**:
  1. Enable Flash Attention:
     .. code-block:: bash

        pip install flash-attn

Advanced Usage
--------------
- **Fine-tuning**:
  .. code-block:: python

     from transformers import TrainingArguments

     training_args = TrainingArguments(
         output_dir="./gemma-finetuned",
         per_device_train_batch_size=4,
         gradient_accumulation_steps=4,
         optim="paged_adamw_8bit"
     )

- **Gradio Interface**:
  .. code-block:: python

     import gradio as gr

     def respond(message):
         inputs = tokenizer(message, return_tensors="pt").to("cuda")
         outputs = model.generate(**inputs, max_new_tokens=100)
         return tokenizer.decode(outputs[0])

     gr.Interface(respond, "textbox", "text").launch()

Further Resources
-----------------
.. seealso::
   - `Gemma Docs <https://ai.google.dev/gemma/docs>`_
   - `Hugging Face Model Card <https://huggingface.co/google/gemma-7b>`_
   - `Quantization Guide <https://huggingface.co/docs/transformers/main/en/quantization>`_