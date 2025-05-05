Getting Started with TensorFlow
===============================

Installing TensorFlow
---------------------

Run:

.. code-block:: shell

   pip install tensorflow

Building a Simple Neural Network
--------------------------------

.. code-block:: python

   import tensorflow as tf
   from tensorflow.keras import layers

   model = tf.keras.Sequential([
       layers.Dense(64, activation='relu', input_shape=(100,)),
       layers.Dense(10, activation='softmax')
   ])

   model.compile(optimizer='adam',
                 loss='sparse_categorical_crossentropy',
                 metrics=['accuracy'])

Training Your Model
-------------------

.. code-block:: python

   model.fit(train_data, train_labels, epochs=5)

Further Resources
-----------------

- `TensorFlow Official Documentation <https://www.tensorflow.org/>`_