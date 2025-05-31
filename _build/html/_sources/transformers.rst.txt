.. _transformers-guide:

NLP with Transformers
=====================

**Purpose**: Implement state-of-the-art natural language processing using Hugging Face Transformers.

Prerequisites
-------------
1. Install required packages:
   .. code-block:: bash

      pip install transformers torch sentencepiece

2. For GPU acceleration (recommended):
   .. code-block:: bash

      pip install cupy-cuda11x  # Match your CUDA version

Step-by-Step Implementation
---------------------------

Step 1: Text Classification
~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from transformers import pipeline

   # Sentiment analysis
   classifier = pipeline("sentiment-analysis")
   result = classifier("I love using Transformers!")
   print(result)  # [{'label': 'POSITIVE', 'score': 0.9998}]

   # Zero-shot classification
   classifier = pipeline("zero-shot-classification")
   result = classifier(
       "This is a tutorial about Python programming",
       candidate_labels=["education", "politics", "business"]
   )
   print(result['labels'][0])  # Most probable label

Step 2: Text Generation
~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   generator = pipeline("text-generation", model="gpt2")

   generated_text = generator(
       "Artificial intelligence is",
       max_length=50,
       num_return_sequences=2,
       temperature=0.7
   )

   for i, seq in enumerate(generated_text):
       print(f"Sequence {i+1}: {seq['generated_text']}")

Step 3: Custom Model Training
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from transformers import AutoTokenizer, AutoModelForSequenceClassification
   from datasets import load_dataset
   import torch

   # Load dataset and model
   dataset = load_dataset("imdb")
   tokenizer = AutoTokenizer.from_pretrained("distilbert-base-uncased")
   model = AutoModelForSequenceClassification.from_pretrained(
       "distilbert-base-uncased",
       num_labels=2
   )

   # Tokenization
   def tokenize(batch):
       return tokenizer(batch["text"], padding=True, truncation=True)

   dataset = dataset.map(tokenize, batched=True)
   dataset.set_format("torch", columns=["input_ids", "attention_mask", "label"])

   # Training setup
   from transformers import Trainer, TrainingArguments

   training_args = TrainingArguments(
       output_dir="./results",
       per_device_train_batch_size=8,
       num_train_epochs=3,
       logging_dir="./logs"
   )

   trainer = Trainer(
       model=model,
       args=training_args,
       train_dataset=dataset["train"].select(range(1000)),  # Small subset
       eval_dataset=dataset["test"].select(range(100))
   )

   trainer.train()

Troubleshooting
---------------
- **OutOfMemoryError**:
  1. Reduce batch size
  2. Use gradient accumulation:
     .. code-block:: python

        training_args = TrainingArguments(
            per_device_train_batch_size=4,
            gradient_accumulation_steps=2
        )

- **Slow performance**:
  1. Enable mixed precision:
     .. code-block:: python

        training_args = TrainingArguments(fp16=True)
  2. Use smaller model (e.g., "distilbert-base-uncased")

- **Tokenization errors**:
  1. Clean text before processing:
     .. code-block:: python

        import re
        text = re.sub(r'[^\w\s]', '', text)

Advanced Usage
--------------
- **Custom Architectures**:
  .. code-block:: python

     from transformers import BertConfig, BertModel

     config = BertConfig(
         hidden_size=768,
         num_attention_heads=12,
         num_hidden_layers=6
     )
     custom_bert = BertModel(config)

- **Model Quantization**:
  .. code-block:: python

     from transformers import AutoModelForSequenceClassification

     quantized_model = AutoModelForSequenceClassification.from_pretrained(
         "distilbert-base-uncased",
         torch_dtype=torch.qint8
     )

Further Resources
-----------------
.. seealso::
   - `Hugging Face Course <https://huggingface.co/course>`_
   - `Transformers Documentation <https://huggingface.co/docs/transformers>`_
   - `Model Hub <https://huggingface.co/models>`_