+++
title = "Creating Text Search for Movie Classifier"

date = 2019-10-07T00:00:00
lastmod = 2019-10-07T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jacob Wilkins"]

tags = ["Information Retrieval"]
summary = "The first step for creating the Movie Classifier is developing the search features. This goes over how I've implemented the TF-IDF, inverted index, etc."

[header]
image = ""
caption = ""

+++

The first step for creating the Movie Classifier is developing the search features. This goes over how I've implemented the TF-IDF, inverted index, etc.

### Deployment Instructions

### Contributions
For text search, I used Toastdriven's microsearch (refer to References) and added some optimizations. Firstly, I added stemming capabilities using the nltk.stem library. In addition, I fixed a bug with the BM25 algorith that resulted in a divide by zero error. For the Flask web app API, I used CoreyMschafer's Flask_Blog repository (refer to References) as a starting point.

### Algorithms Explained

### Test Cases

### References
I based the text search algorithm on this code: https://github.com/toastdriven/microsearch. 
I used this code: https://github.com/CoreyMSchafer/code_snippets/tree/master/Python/Flask_Blog as a starting point for the development of the web app.
