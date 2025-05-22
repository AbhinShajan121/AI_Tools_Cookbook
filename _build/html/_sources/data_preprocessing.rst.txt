Data Preprocessing for AI
=========================

Quality data is critical for building effective AI models.

Common Steps:

- Cleaning data (handling missing or inconsistent data)
- Normalization and scaling
- Encoding categorical variables
- Feature selection and extraction

Example: Using Pandas and Scikit-learn

.. code-block:: python

   import pandas as pd
   from sklearn.preprocessing import StandardScaler

   # Load data
   df = pd.read_csv('data.csv')

   # Fill missing values
   df.fillna(method='ffill', inplace=True)

   # Scale numerical features
   scaler = StandardScaler()
   df[['feature1', 'feature2']] = scaler.fit_transform(df[['feature1', 'feature2']])

Further Resources
-----------------

- `Pandas Documentation <https://pandas.pydata.org/docs/>`_
- `Scikit-learn Preprocessing <https://scikit-learn.org/stable/modules/preprocessing.html>`_