+++
title = "Creating Image Caption Generator for Movifier"

date = 2019-10-28T00:00:00
lastmod = 2019-10-28T00:00:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Jacob Wilkins"]

tags = ["Information Retrieval"]
summary = "The third step for creating Movifier is developing the image caption generator. This goes over how I've implemented movie poster caption generation."

[header]
image = ""
caption = ""

+++

The third step for creating **[Movifier](https://github.com/JacobAWilkins/Movifier)** is developing an image caption generator. Given **[this](https://www.kaggle.com/rounakbanik/the-movies-dataset)** dataset of movie titles and descriptions from Kaggle, Movifier implements text search using Okapi BM25 to score, classifies movies by genre, and generates captions for movie scenes using the **[Flicker8k](http://academictorrents.com/details/9dea07ba660a722ae1008c4c8afdd303b6f6e53b)** dataset to train. The text search takes a description of a movie and outputs a list of similar movies with similar descriptions. 

Movifier is developed using **[Flask](https://www.fullstackpython.com/flask.html)**, a lightweight WSGI web application framework. The project proposal can be found **[here](https://docs.google.com/document/d/1uDnyLfvAJTHSIp2gLQYVDAONRQX91yI2uVtycHrf1pE/edit?usp=sharing)**.

### Deployment Instructions
##### Online
http://jacobwilkins.pythonanywhere.com/caption
##### Localhost
1. Install Flask:
```pip install flask```
2. Run Movifier:
```sudo python main.py```
   Wait for the dataset to index movies. This will take a while unless you change the number of movies indexed to a lower number. The default is 1000. You can change this in main.py. Afterward, you will see a message ``` * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)``` in the terminal.
3. In a browser (localhost, port 5000):
```http://127.0.0.1:5000/caption```

### Contributions & References
To develop my image caption generator I used **[this](https://www.tensorflow.org/tutorials/text/image_captioning)** TensorFlow tutorial. The Flickr8k Dataset used for training the deep learning model was torrented from **[here](http://academictorrents.com/details/9dea07ba660a722ae1008c4c8afdd303b6f6e53b)** and stored in **[this](https://drive.google.com/open?id=1V3xKf69SHmuXMiGzrBc_oiJcJshyoyIG)** Google Drive for programmatic access.
```
if not os.path.exists(os.path.abspath('.') + '/' + 'Flicker8k_Dataset/'):
gdd.download_file_from_google_drive(file_id='1V3xKf69SHmuXMiGzrBc_oiJcJshyoyIG',
                                    dest_path='./Flickr8k_Dataset.zip',
                                    unzip=True)
```
To contribute to the effictiveness of the caption generator I used the tensorflow.data.Dataset library to map the dataset into numpy files.

### Algorithm Explained
##### Important Libraries
* [tensorflow](https://www.tensorflow.org/)
* [sklearn.model_selection.train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)
* [sklearn.utils.shuffle](https://scikit-learn.org/stable/modules/generated/sklearn.utils.shuffle.html)
* [PIL.Image](https://pillow.readthedocs.io/en/stable/reference/Image.html)
* [GoogleDriveDownloader](https://www.pydoc.io/pypi/googledrivedownloader-0.3/autoapi/google_drive_downloader/index.html)
##### InceptionV3
A deep learning model is preprocessed, built, and trained using **[InceptionV3](https://www.tensorflow.org/api_docs/python/tf/keras/applications/InceptionV3)** (from TensorFlow's keras API). The image features are then extracted from the model using new inputs and a hidden layer to generate the caption.
```
image_model = tf.keras.applications.InceptionV3(include_top=False, weights='imagenet')
new_input = image_model.input
hidden_layer = image_model.layers[-1].output
self.image_features_extract_model = tf.keras.Model(new_input, hidden_layer)
```
Max length is determined for the attention weights. The captions are tokenized into vector sequences which are padded based on the max length. The other features are determined using the embedding dimensions and units size.

### Challenge
One major issue I ran into was trying to find a way to access the files online in a programmatic way. First, I tried uploading the image dataset to GitHub, but I had trouble download the files from there. Next, I uploaded the images to Google Photos, but then soon realized there was no way to access them as all the filenames had been changed during the upload. Finally, I decided to upload the dataset to Google Drive and used the [GoogleDriveDownloader](https://www.pydoc.io/pypi/googledrivedownloader-0.3/autoapi/google_drive_downloader/index.html) API to easily access the images.
