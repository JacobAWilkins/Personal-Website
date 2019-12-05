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

The second step for creating **[Movifier](https://github.com/JacobAWilkins/Movifier)** is developing movie classifer. Given **[this](https://www.kaggle.com/rounakbanik/the-movies-dataset)** dataset of movie titles and descriptions from Kaggle, Movifier implements text search using Okapi BM25 to score, classifies movies by genre, and generates captions for movie scenes using the **[Flicker8k](http://academictorrents.com/details/9dea07ba660a722ae1008c4c8afdd303b6f6e53b)** dataset to train. The text search takes a description of a movie and outputs a list of similar movies with similar descriptions. 

Movifier is developed using **[Flask](https://www.fullstackpython.com/flask.html)**, a lightweight WSGI web application framework. The project proposal can be found **[here](https://docs.google.com/document/d/1uDnyLfvAJTHSIp2gLQYVDAONRQX91yI2uVtycHrf1pE/edit?usp=sharing)**.

The project demo can be viewed **[here](https://youtu.be/DVKHCncddg8)**.

### Deployment Instructions
##### Online
http://jacobwilkins.pythonanywhere.com/classifer
##### Localhost
1. Install Flask:
```pip install flask```
2. Run Movifier:
```sudo python main.py```
   Wait for the dataset to index movies. This will take a while unless you change the number of movies indexed to a lower number. The default is 1000. You can change this in main.py. Afterward, you will see a message ``` * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)``` in the terminal.
3. In a browser (localhost, port 5000):
```http://127.0.0.1:5000/classifer```

### Contributions & References
For the test classifier I used **[this](https://stackabuse.com/text-classification-with-python-and-scikit-learn/)** stackabuse article and **[this](https://github.com/ishmeetkohli/imdbGenreClassification/blob/master/utils.py)** GitHub repository as reference. Both of these solutions used a train test split to test the accuracy of the algorithm, but I modified it to only classify the data inputted by the user. I added to these references by transforming them into a system that uses train data to classify a single test case. I made the train and test data identical in form and used pandas to make Data Frames for the test/train allowing them to both have the same number of data features.

### Algorithms Explained
##### Important Libraries
* [CountVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html)
* [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)
* [WordNetLemmatizer](https://www.geeksforgeeks.org/python-lemmatization-with-nltk/)
* [pandas](https://pandas.pydata.org/pandas-docs/stable/index.html)
##### Preprocess
Before classification begins, the movie descriptions of the train/test data are preprocessed to remove all special characters, remove all single characters, remove all multiple spaces for singles spaces, and converted to lowercase. Finally, lemmatization is performed using nltk's [WordNetLemmatizer](https://www.geeksforgeeks.org/python-lemmatization-with-nltk/).
##### Count Vectorizer
The [CountVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html) fitted and transformed the text descriptions of the train/test data (the train movie decriptions) to identify the data features and then converted them into an array to be used for classification.
```
vectorizer = CountVectorizer(analyzer="word", tokenizer=None, preprocessor=None, stop_words=None, max_features=3000)
train_data_features = vectorizer.fit_transform(trainData['plot'])
train_data_features = train_data_features.toarray()
```
##### Random Forest Classifier
The [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) fitted the data features to the movies genres and then used this to make a prediction of the genre for the test data. The prediction is displayed for the user.
```
classifier = RandomForestClassifier(n_estimators=1000, random_state=0)
classifier = classifier.fit(train_data_features, trainData['tags'])
```

### Challenge
My challenge was to transform the references that were designed for test train split into a system that used train data to classify a single test case. I resolved this issue by making the train and test data identical in form and used [pandas](https://pandas.pydata.org/pandas-docs/stable/index.html) to make Data Frames for the test/train allowing them to both have the same number of data features.
