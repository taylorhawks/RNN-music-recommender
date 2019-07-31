# Recommender
Ordered recommendations using recurrent nerual networks.

The main focus of this project is a content-based algorithm.

## The Algorithm
1. Body of songs is selected using collaborative matrix completion.
2. Recurrent neural network determines the ideal feature vector for the next song based on the previous sequence of songs.
3. The next song is selected based on minimum cost, which is determined based on the distance from a song to ideal feature vector as well as the consonance of song key transition.
4. Next song is plugged into the RNN and the process repeats from step 2.

## Key Concepts
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

## Outline
#### MVP
- Get playlist and song data from spotify API
- Train and evaluate RNN
- PCA and visualization
- Functional input/output

#### Optional
- API with I/O
- Key considerations (see quantifying dissonance)
- Genre considerations
- Dimensionality reduction
- Try different distance metrics
  
 #### Very optional
 - 3D Animated Visualization
 - Combine with collaborative factors
