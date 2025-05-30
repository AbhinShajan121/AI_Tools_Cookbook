Model Evaluation and Tuning
===========================

Evaluating your AI models is essential to ensure good performance.

Common Techniques:

- Train/Test Split
- Cross-Validation
- Metrics (Accuracy, Precision, Recall, F1 Score)
- Hyperparameter Tuning (Grid Search, Random Search)

Example: Using Scikit-learn

.. code-block:: python

   from sklearn.model_selection import train_test_split, GridSearchCV
   from sklearn.metrics import accuracy_score
   from sklearn.svm import SVC

   X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

   model = SVC()

   parameters = {'kernel':('linear', 'rbf'), 'C':[1, 10]}

   clf = GridSearchCV(model, parameters)
   clf.fit(X_train, y_train)

   predictions = clf.predict(X_test)
   print('Accuracy:', accuracy_score(y_test, predictions))

Further Resources
-----------------

.. seealso::

Scikit-learn Model Evaluation <https://scikit-learn.org/stable/modules/model_evaluation.html>