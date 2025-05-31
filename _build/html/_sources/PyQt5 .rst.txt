
Building AI Interfaces with PyQt5
=================================

**Purpose**: Create desktop applications with interactive AI features.

Prerequisites
-------------
1. Install PyQt5:
   .. code-block:: bash

      pip install PyQt5

2. Optional (for styling):
   .. code-block:: bash

      pip install pyqt5-tools

Step-by-Step Implementation
---------------------------

Step 1: Create a Basic Window
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel

   class AIWindow(QMainWindow):
       def __init__(self):
           super().__init__()
           self.setWindowTitle("AI Tool")
           self.setGeometry(100, 100, 400, 200)
           
           # Add a label
           self.label = QLabel("Welcome to your AI App", self)
           self.label.move(50, 50)
           
   if __name__ == "__main__":
       app = QApplication([])
       window = AIWindow()
       window.show()
       app.exec_()

Step 2: Add AI Functionality
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from PyQt5.QtWidgets import QPushButton, QTextEdit
   import ollama  # Example AI integration

   class AIWindow(QMainWindow):
       def __init__(self):
           # ... (previous code)
           
           # Add interactive elements
           self.button = QPushButton("Ask AI", self)
           self.button.move(50, 100)
           self.button.clicked.connect(self.run_ai)
           
           self.text_area = QTextEdit(self)
           self.text_area.setGeometry(50, 150, 300, 200)
       
       def run_ai(self):
           """Query local LLM when button is clicked"""
           response = ollama.chat(
               model="mistral",
               messages=[{"role": "user", "content": self.text_area.toPlainText()}]
           )
           self.label.setText(response['message']['content'])

Step 3: Style Your App (Optional)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. code-block:: python

   from PyQt5.QtGui import QFont

   # Inside __init__():
   self.setStyleSheet("background-color: #f0f0f0;")
   self.label.setFont(QFont("Arial", 14))
   self.button.setStyleSheet("""
       QPushButton {
           background-color: #4CAF50;
           color: white;
           border: none;
           padding: 8px 16px;
       }
       QPushButton:hover {
           background-color: #45a049;
       }
   """)

Troubleshooting
---------------
- **Blank window appears**:
  1. Verify `app.exec_()` is called
  2. Check for errors in terminal

- **Buttons not responding**:
  1. Confirm `connect()` points to valid method
  2. Ensure no exceptions in callback functions

- **High CPU usage**:
  - Use threading for long-running AI tasks:
    .. code-block:: python

       from threading import Thread
       Thread(target=self.run_ai).start()

Advanced Features
-----------------
- **Drag-and-Drop AI Processing**:
  .. code-block:: python

     from PyQt5.QtCore import Qt

     class AIWindow(QMainWindow):
         def __init__(self):
             # ...
             self.setAcceptDrops(True)
         
         def dragEnterEvent(self, event):
             if event.mimeData().hasUrls():
                 event.accept()
         
         def dropEvent(self, event):
             file_path = event.mimeData().urls()[0].toLocalFile()
             # Process file with AI

Further Resources
-----------------
.. seealso::
   - `PyQt5 Documentation <https://www.riverbankcomputing.com/static/Docs/PyQt5/>`_
   - `Qt Style Sheets Reference <https://doc.qt.io/qt-5/stylesheet-reference.html>`_
   - `Ollama Python API <https://github.com/ollama/ollama-python>`_