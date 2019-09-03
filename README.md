# The Best Playlists are More than a Collection of Songs
Have you ever made a playlist or mixtape and are stuck on the order to put the songs in?  Maybe we can learn from different spotify users what makes a good playlist.
The best playlists have a good flow.  This is what separates a good DJ from a bad DJ, given they have the same tracks and technical aptitude.  Build-ups and break-downs make for an interesting experience, and it’s more than just picking the most similar song to the last one.

### The Solution
_Deep Sequential Content Optimization_ or "DISCO"
- Ordered recommendations using recurrent nerual networks.
- The main focus of this project is a content-based algorithm that would sit on top of a layer of collaborative filtering.

## Table of Contents - Highlights
- pipeline.ipynb - This is the algorithm in action with a full pipeline of transformations and predictions to build playlists.
- /cloud/model.ipynb - RNN trained on Amazon SageMaker
- /data-wrangling/preprocessing.ipynb - the majority of data preprocessing and EDA is here.  This is also where PCA and scalers are trained.

## The Algorithm
1. A sub-set of songs is selected using collaborative filtering or a simple query based on subgenre.  I'm using Spotify's Api to select roughly 200-400 songs.
2. A recurrent neural network determines the ideal feature vector for the next song based on the previous sequence of songs.
3. The next song is selected based on minimum loss from the sub-set selected in step 1.  The loss function is determined based on the distance from a song to the ideal feature vector as well as the consonance of song key transition and similarity of tempo. This is a greedy algorithm which does not consider whether the song might better fulfill the objective function better later in the sequence.
4. Next song is plugged into the RNN and the process repeats from step 2 until the playlist is a satisfactory length.

---
## The Data
- Inital Data...
  - 15,918 users
  - 157,504 playlists
  - 2,032,044 songs
- The data used
  - Very large and very small playlists removed
  - Things like “liked from radio” dropped
- Used that to build search strings and hit spotify’s API for like literally a week straight
- Training Data for RNN is a 72051 x 50 x 9 tensor

<img src = "images/distplot.png"/>

## Features
_Metadata from Spotify "Features" API_

Concrete Features
- Key
- Mode
- Tempo
“Abstract” Features
- Acousticness
- Danceability
- Energy
- Instrumentalness
- Liveness
- Loudness
- Speechiness
- Valence

<img src= "images/heatmap.png">

---
# Recurrent Neural Network
A recurrent neural network is different from other deep learning architectures because it learns sequences rather than a single set of values.  While RNN applications in recommendation systems typically involve one-hot encoding for the next item in a sequence, I've employed RNNs for multivariate time series forecasting of the different "abstract features" which describe the character of songs in a playlist.  The RNN architecture is 9 inputs, 8 outputs, with two 16-node hidden layers.  8 input/output nodes correspond to the 8 "abstract features," and one additional one is used in the input layer for mode. (More on this later.) The model's mean absolute error is 0.5848 and the mean absolute deviation in the training data is 0.8535.

<img src="images/architecture.png"/>

The model uses a many-to-many sequence learning format, and in its implementation is used as many-to-one, where the output is not fed back into the input (without some modification... more on that in the next section).  At each step of the RNN, the whole computation graph (above) is used.

<img src="images/many-to-one.png"/>

Standard Scaler and Yeo-Johnson Power Transformation applied to training set with duplicates removed, to give the data better distributions both for training as well as distance metrics.  Furthermore, some features, especially "Loudness," benefit from reducing the extreme long tails.

Although Euclidian distance is ideal for model implementation, MSE often leads to under-estimation of weights and biases as gradients lead to local minima near zero, as outliers are heavily penalized.  This is why MAE is used as an objective function instead.

Linear activations were used in all layers as they are less likely to under-estimate features and produce a higher-variance model.  Weights are initialized randomly, and Adam optimizer was used instead of RMSProp, though the latter is more common for RNNs.  The logic gates of GRU and LSTM are not necessary as long-term dependency is not a major concern.




---

#### The RNN
<img src = "/images/rnn_instance.png"/>

#### Selecting the next song
_Find a balanced minimum of distance, dissonance, and tempo change._
<img src = "/images/song_selection_u.png"/>

#### Computing with the RNN
<img/>

## Key Concepts
- Recommendation Systems
- Sequence Learning
- Recurrent Neural Networks
- Computational Music Theory
- Traveling Salesman Problem

## Neural Network Architecture
- Recurrent
- Unidirectional
- Many-to-one architecture (or a somewhat modified many-to-many)
- tanh or linear activation in hidden layers
- Sigmoid activation in output layer
- GRU is not necessary because long-term dependency is not important.
- Similarly, LSTM is not necessary.
- Conventions:
  - 2 or 3 hidden layers (more than that is quite rare for RNN due to the temporal dimension)
  - Sometimes start with recurrent layers, and _then_ have deep layers.

## Research
- Sequence-Aware Recommender Systems: https://arxiv.org/pdf/1802.08452.pdf
- https://papers.nips.cc/paper/5653-a-recurrent-latent-variable-model-for-sequential-data.pdf
- https://www.kdnuggets.com/2015/06/rnn-tutorial-sequence-learning-recurrent-neural-networks.html
- https://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks.pdf
- Sequence Learning from Nvidia: https://devblogs.nvidia.com/deep-learning-nutshell-sequence-learning/
- The GOAT Andrew Ng: https://www.coursera.org/learn/nlp-sequence-models/lecture/ftkzt/recurrent-neural-network-model
- Jason Brownlee: https://machinelearningmastery.com/sequence-prediction/
- Music consonance and dissonance: https://music.stackexchange.com/questions/4439/is-there-a-way-to-measure-the-consonance-or-dissonance-of-a-chord
- Quantifying dissonance: http://musicalgorithms.ewu.edu/learnmoresra/files/vassilakis2005sre.pdf
- http://sethares.engr.wisc.edu/comprog.html
- https://arxiv.org/pdf/1603.08904.pdf
- https://www.oxfordscholarship.com/view/10.1093/acprof:oso/9780195148367.001.0001/acprof-9780195148367-chapter-7
- https://i.stack.imgur.com/wmT4w.png
- kind-of famous blog post: http://karpathy.github.io/2015/05/21/rnn-effectiveness/
- drawing diagrams:
  - http://fastml.com/deep-learning-architecture-diagrams/
  - https://datascience.stackexchange.com/questions/14899/how-to-draw-deep-learning-network-architecture-diagrams
- http://docs.echonest.com.s3-website-us-east-1.amazonaws.com/_static/AnalyzeDocumentation.pdf
- https://plot.ly/python/animations/

#### More on Key Transition Consonance
- http://mindmodeling.org/cogsci2011/papers/0843/paper0843.pdf
- This might be the one: https://arxiv.org/pdf/1603.08904.pdf
  
## Data Gathering
#### Spotify API
- How to get playlists: https://developer.spotify.com/documentation/web-api/reference/browse/get-list-featured-playlists/
- Playlist curators: https://sidekick-music.com/2018/11/30/top-spotify-playlist-curators-2018/
- Collaborative part:
  - Already have a dataset of users playlists
  - Can also use spotify's built-in recommendation via the API (as a placeholder of course)


## Outline
#### MVP
- Get playlist and song data from spotify API
- EDA w/ Dickey-Fuller test for stationarity of time series principal components
- Train and evaluate RNN
- PCA and visualization
- Functional input/output
- Neural network diagrams

#### Optional
- API with I/O
- Key considerations (see quantifying dissonance)
- Genre considerations
- Dimensionality reduction
- Try different distance metrics
  
 #### Very optional
 - 3D Animated Visualization (Plotly)
 - Collaborative filtering
 
 ## Tech
 - Spotify API
 - Keras
 - Plotly
 - Mlrose
 - Selenium ?
