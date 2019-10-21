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
Project Proposal: https://docs.google.com/document/d/1uDnyLfvAJTHSIp2gLQYVDAONRQX91yI2uVtycHrf1pE/edit?usp=sharing

### Deployment Instructions
##### Online
http://jacobwilkins.pythonanywhere.com/home
##### Localhost
1. Install Flash:
```pip install flask```
2. Run Movifier:
```sudo python main.py```
   Wait for dataset to index movies. This will take a while unless you change the number of movies indexed to a lower number. The default is 1000. You can change this in main.py. Afterwards, you will see a message ``` * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)``` in the terminal.
3. In browser (localhost, port 5000):
```http://127.0.0.1:5000/```

### Contributions
For text search, I used Toastdriven's microsearch (refer to References) and added some optimizations. Firstly, I added stemming capabilities using the nltk.stem library. In addition, I added my own Okapi BM25 alogrithm based on the formulas from Wikipedia (refer to References).
For the Flask web app API, I used CoreyMschafer's Flask_Blog repository (refer to References) as a starting point.

### Algorithms Explained
##### Documents
A field-based dictionary where the keys are field names and the values are the field's contents.
```
{
  "id": "movie-title",
  "text": "This is a movies description",
}
```
##### Inverted Index
A term-based dictionary where the keys are terms and the values are documents/position information.
```
index = {
        'happy': {
            'Toy Story': [3],
        },
        'back': {
            'Terminator': [5, 10],
        },
        ...
    }
```
##### Okapi BM25
For a given document, the BM25 relevance is calculated as
```
def bm25_relevance(self, terms, matches, current_doc, total_docs, curr_len, avg_len, b, k):
        score = 0

        for term in terms:
            idf = math.log((total_docs - matches[term] + 0.5) / (matches[term] + 0.5))
            score = score + (current_doc.get(term, 0) * idf * (k + 1)) / (current_doc.get(term, 0) + k * (1 - b + (b * (curr_len / avg_len))))

        return score
```
where "terms" is a list of terms, "matches" is the first dictionary returned from collect_results(self, terms), "current doc" is the second dictionary returned from collect_results(self, terms), "total_docs" is the total number of documents in the index, "curr_len" is the length of the current document, and "avg_len" is the average length of all the documents. "b" and "k" are used to modify scores to fall in a given range.
##### Other Optimizations
Ngrams:
Front n-grams of tokens are made from 3 to 6 in gram length
```
terms = {}
for position, token in enumerate(tokens):
  for window_length in range(min_gram, min(max_gram + 1, len(token) + 1)):
    gram = token[:window_length]
    terms.setdefault(gram, [])
    if not position in terms[gram]:
       terms[gram].append(position)
return terms
```
Filter stop words:
```
stopwords = set([
        'a', 'an', 'and', 'are', 'as', 'at', 'be', 'but', 'by',
        'for', 'if', 'in', 'into', 'is', 'it',
        'no', 'not', 'of', 'on', 'or', 's', 'such',
        't', 'that', 'the', 'their', 'then', 'there', 'these',
        'they', 'this', 'to', 'was', 'will', 'with'
    ])
```
Filter punctuation:
```
punctuation = re.compile('[~`!@#$%^&*()+={\[}\]|\\:;"\',<.>/?]')
```
Stemming:
```
ps = PorterStemmer()
token = ps.stem(token)
```
### Test Cases

### References
I based the text search algorithm on this code: https://github.com/toastdriven/microsearch. 
I used this code: https://github.com/CoreyMSchafer/code_snippets/tree/master/Python/Flask_Blog as a starting point for the development of the web app.
I used this to develop my Okapi BM25 algorithm: https://en.wikipedia.org/wiki/Okapi_BM25
