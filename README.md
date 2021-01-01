# Fourier Features for Super Resolution
Implicit neural networks try to represent natural signals by approximating a function that maps the input coordinates to the signal values. But because of "spectral bias" they cannot capture high-frequency details [[1](#References)]. 

Fortunately, if we featurize the input coordinates with Fourier features, the network becomes an effective interpolation machine and we can successfully parameterize the signal [[2](#References)][[3](#References)]. This interpolation behavior should be useful for super-resolution tasks. **Fourier Feature Networks** [[2](#References)] demonstrate this for an image superresolution task (called “image regression” in the paper). I wanted to try it out for other natural signals like audio and video super-resolution. It also gives me an opportunity to reimplement the basic idea of the paper.

## Fourier Feature of Coordinates
If x is the vector of coordinates, then their corresponding Fourier features are calculated as:\
`FF(x) = [sin(2*pi*B @ x), cos(2*pi*B @ x)]`\
Where the entries of the B matrix are sampled randomly from a Gaussian distribution. The "mean" of the distribution is set to 0 and the "std" is left as a hyperparameter(called `scale`).
The `scale` parameter determines the range of the frequencies that can be learned by the network, so it has to be manually chosen for each type of signal. Setting too high a value for `scale` causes high-frequency noise in the output and setting too low a value causes over-smoothed output [[2](#References)].

## Image SuperRes: 
### [ImageSuperRes.ipynb](ImageSuperRes.ipynb) | [Google Colab](https://colab.research.google.com/drive/1XfSUIKOCV7VvA3bilnO33lw_-Nx1j3Te?usp=sharing)

It is the same as the “image regression” task in the paper. The network is trained on every other pixel, and at test time it is evaluated on all the pixels. So this is essentially a 2x spatial superresolution task.\
Following the paper, `scale=10` value was chosen as a hyperparameter. 


## Audio SuperRes:
### [AudioSuperRes.ipynb](AudioSuperRes.ipynb) | [Google Colab](https://colab.research.google.com/drive/1mOdmm2l-KOCrAZEwE7k4ABGG0kGf_DNL?usp=sharing)

The network is trained on every other frame of a stereo audio, and at test time it is evaluated on all the frames. So it is essentially a 2x temporal superresolution task.\
Since an audio signal contains many high-frequency contents, the `scale` parameter had to be set at a very high value. Here, `scale=5000` was manually chosen for a particular piano recording.


## Video SuperRes:
### [VideoSuperRes.ipynb](VideoSuperRes.ipynb) | [Google Colab](https://colab.research.google.com/drive/1SzDyh0XbIEVHj3NmWoAJUi-xN837DLBL?usp=sharing)

The network is trained on every other video frame and every other pixel of those frames. At test time it is evaluated on all the pixels in the video. So, it is essentially a 2x Spatio-temporal super-resolution task (increase the frame rate by 2x, also increase frame resolutions by 2x).\
Here, `scale=5` was manually chosen for a particular test video.

# References
1. [On the Spectral Bias of Neural Networks](https://arxiv.org/abs/1806.08734), Rahaman et al. 2019
2. [Fourier Features Let Networks Learn High Frequency Functions in Low Dimensional Domains](https://people.eecs.berkeley.edu/~bmild/fourfeat/), Tancik et al. 2020
3. [Understanding and Extending Neural Radiance Fields](https://youtu.be/nRyOzHpcr4Q), Jon Barron, TUM AI Lecture Series
