# Recommender
Ordered recommendations using recurrent nerual networks.

The main focus of this project is a content-based algorithm.

## The Algorithm
1. A sub-set of songs is selected using either collaborative filtering or a simple query based on subgenre.  This step is open to experimentation and is not the main scope of focus for this project.
2. A recurrent neural network determines the ideal feature vector for the next song based on the previous sequence of songs.  This is a simple RNN without GRU or LSTM.
3. The next song is selected based on minimum loss from the sub-set selected in step 1.  The loss function is determined based on the distance from a song to the ideal feature vector as well as the consonance of song key transition (and possilby other aspects related to computational music theory).  This is a greedy algorithm which does not consider whether the song might better fulfill the objective function better later in the sequence.
4. Next song is plugged into the RNN and the process repeats from step 2.
5. Once the RNN is fully trained the algorithm will be able to create playlists given a small number of songs as a starting sequence - even just a single song.

#### Selecting the next song:
<img src = "/images/song_selection_u.png"/>

#### Computing with the RNN:
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
  

## Outline
#### MVP
- Get playlist and song data from spotify API
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
