+++
title = "Creating Movie Classifier for Movifier"

date = 2019-10-28T00:00:00
lastmod = 2019-10-28T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jacob Wilkins"]

tags = ["Information Retrieval"]
summary = "The second step for creating Movifier is developing the movie classifer. This goes over how I've implemented movie description classification by genre."

[header]
image = ""
caption = ""

+++

The second step for creating **[Movifier](https://github.com/JacobAWilkins/Movifier)** is developing movie classifer. Given **[this](https://www.kaggle.com/rounakbanik/the-movies-dataset)** dataset of movie titles and descriptions from Kaggle, Movifier implements text search using Okapi BM25 to score, classifies movies by genre, and generates captions for movie scenes. The text search takes a description of a movie and outputs a list of similar movies with similar descriptions. 

Movifier is developed using **[Flask](https://www.fullstackpython.com/flask.html)**, a lightweight WSGI web application framework. The project proposal can be found **[here](https://docs.google.com/document/d/1uDnyLfvAJTHSIp2gLQYVDAONRQX91yI2uVtycHrf1pE/edit?usp=sharing)**.

### Deployment Instructions
##### Online
http://jacobwilkins.pythonanywhere.com/classifer
##### Localhost
1. Install Flask:
```pip install flask```
2. Run Movifier:
```sudo python main.py```
   Wait for dataset to index movies. This will take a while unless you change the number of movies indexed to a lower number. The default is 1000. You can change this in main.py. Afterwards, you will see a message ``` * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)``` in the terminal.
3. In browser (localhost, port 5000):
```http://127.0.0.1:5000/classifer```

### Contributions & References
For the test classifer I used **[this](https://stackabuse.com/text-classification-with-python-and-scikit-learn/)** stackabuse article and **[this](https://github.com/ishmeetkohli/imdbGenreClassification/blob/master/utils.py)** GitHub repository as reference. Both of these solutions used a train test split to test the accuracy of the algorithm, but I modified it to only classify the data inputed by the user.

### Algorithms Explained
A few of the most important libraries used were CountVectorizer, RandomForestClassifier, WordNetLemmatizer, and pandas.
##### Preprocess
Before classification begins, the movie descriptions of the train/test data are preprocessed to remove all special characters, remove all single characters, remove all multiple spaces for singles spaces, and converted to lowercase. Finally, lemmantixation is performed.
##### Count Vectorizer
The vectorizer fitted and transformed the text descriptions of the train/test data (the train movie decriptions) to identify the data features and then converted them into an array to be used for classification.
##### Random Forest Classifier
The classifier fitted the data features to the movies genres and then used this to make a prediction of the genre for the test data. The prediction is displayed for the user.

### Challenge
My challenge was to transform the references that were designed for test train split into a system that used train data to classify a single test case. I resolved this issue by making the train and test data identical in form and used pandas to make Data Frames for the test/train allowing them to both have the same number of data features.
