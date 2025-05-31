.. _yolov8-guide:

Object Detection with YOLOv8
============================

**Purpose**: Train and deploy state-of-the-art object detection models.

Prerequisites
-------------
1. Install required packages:
   .. code-block:: bash

      pip install ultralytics torch torchvision

2. Verify installation:
   .. code-block:: python

      import ultralytics
      ultralytics.checks()

Step-by-Step Implementation
---------------------------

Step 1: Basic Inference
~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from ultralytics import YOLO

   # Load pretrained model
   model = YOLO('yolov8n.pt')  # nano version

   # Run inference
   results = model('street.jpg')

   # Display results
   results[0].show()

Step 2: Custom Training
~~~~~~~~~~~~~~~~~~~~~~~
1. Prepare dataset in YOLO format:
    dataset/
    ├── images/
    │ ├── train/
    │ └── val/
    └── labels/
    ├── train/
    └── val/

2. Training script:
.. code-block:: python

   model = YOLO('yolov8s.pt')  # small version

   results = model.train(
       data='coco128.yaml',
       epochs=100,
       imgsz=640,
       batch=16,
       name='custom_model'
   )

Step 3: Model Export
~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

# Export to ONNX
model.export(format='onnx')

# Export to TensorRT (requires GPU)
model.export(format='engine', device=0)

Troubleshooting
---------------
- **CUDA Out of Memory**:
1. Reduce batch size:
  .. code-block:: python

     model.train(batch=8)
2. Use smaller model (yolov8n instead of yolov8x)

- **Poor Detection Quality**:
1. Increase epochs (50-300)
2. Add more training data
3. Adjust learning rate:
  .. code-block:: python

     model.train(lr0=0.01, lrf=0.1)

- **Slow Inference**:
1. Export to TensorRT
2. Use FP16 precision:
  .. code-block:: python

     model.export(half=True)

Advanced Usage
--------------
- **Video Processing**:
.. code-block:: python

  results = model.track(
      'input.mp4',
      show=True,
      tracker='bytetrack.yaml'
  )

- **Custom Architectures**:
.. code-block:: yaml

  # yolov8-custom.yaml
  backbone:
    - [-1, 1, Conv, [64, 3, 2]]
  head:
    - [-1, 1, Detect, [nc]]

- **Integration with CVAT**:
1. Export annotations as YOLO format
2. Use for training:
  .. code-block:: python

     model.train(data='cvat_export.yaml')

Further Resources
-----------------
.. seealso::
- `Ultralytics Documentation <https://docs.ultralytics.com>`_
- `YOLOv8 GitHub <https://github.com/ultralytics/ultralytics>`_
- `Pretrained Models <https://github.com/ultralytics/assets/releases>`_