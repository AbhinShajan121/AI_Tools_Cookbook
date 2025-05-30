Using AI Cloud Services
=======================

Overview
--------

Many cloud providers offer AI services that let you integrate AI capabilities without deep ML expertise.

Popular Providers:

- Google Cloud AI
- Microsoft Azure Cognitive Services
- Amazon AWS AI Services
- OpenAI API

Getting Started
---------------

Explore each provider’s documentation for setup and API usage.

Example: Using OpenAI API

.. code-block:: python

   import openai

   openai.api_key = 'your-api-key'

   response = openai.Completion.create(
       engine="text-davinci-003",
       prompt="Write a haiku about nature",
       max_tokens=20
   )

   print(response.choices[0].text.strip())

Further Resources
-----------------
.. seealso::

Google Cloud AI <https://cloud.google.com/products/ai>
Microsoft Azure Cognitive Services <https://azure.microsoft.com/services/cognitive-services/>
Amazon AI Services <https://aws.amazon.com/machine-learning/>
OpenAI API <https://openai.com/api/>