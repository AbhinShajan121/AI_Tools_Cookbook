Documentation Guide
===================

Overview
--------

This section explains how the AI Tools Cookbook documentation is organized and how you can build and contribute to it.

Structure
---------

- The source files are written in reStructuredText (.rst) format.
- The main entry point is `index.rst`.
- Additional chapters and recipes are individual `.rst` files linked through the table of contents.

Building the Documentation
--------------------------

To generate the HTML documentation, run the following command in the project root directory:

.. code-block:: shell

   make html

On Windows, you can also run:

.. code-block:: shell

   .\make.bat html

The HTML files will be generated in the `_build/html` directory. Open `index.html` there to view the documentation in your web browser.

Editing the Documentation
-------------------------

- Edit the `.rst` files inside the project folder to update or add content.
- Use proper reStructuredText syntax for formatting.
- To add a new chapter or recipe, create a new `.rst` file and add it to the `.. toctree::` directive in `index.rst`.

Contributing
------------

If you want to contribute to this cookbook:

1. Fork the repository.
2. Create a new branch for your changes.
3. Add or update `.rst` files.
4. Build the documentation locally to check formatting.
5. Submit a pull request with your changes.

Resources
---------

- `Sphinx Documentation <https://www.sphinx-doc.org/>`_ – Learn more about Sphinx and reStructuredText.
- `reStructuredText Primer <https://docutils.sourceforge.io/rst.html>`_ – Quick guide to writing `.rst` files.
