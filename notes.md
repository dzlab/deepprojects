### Segmentation
- Full pipeline demo: poly -> pixels -> ML -> poly - [link](https://www.kaggle.com/lopuhin/full-pipeline-demo-poly-pixels-ml-poly)
- Vision for driving - [link](https://wayve.ai/blog/2018/10/8/vision-for-driving-with-deep-learning)

### Face Detection
- Using Dlib [python](https://engmrk.com/face-detection-application/) [javascript](https://medium.com/@muehler.v/node-js-face-recognition-js-simple-and-robust-face-recognition-using-deep-learning-ea5ba8e852)
0 Using Chrome FaceDetection API [sample](https://medium.com/@joomiguelcunha/lets-play-with-chrome-s-face-detection-api-ca13017a958f)

### Audio
- Web Audio ML Features - [link](https://smus.com/web-audio-ml-features/)
- Audio processing applications - [link](https://www.analyticsvidhya.com/blog/2018/01/10-audio-processing-projects-applications/)
- Speech Recognition Engine using ConvNet in Keras - [link](https://blog.manash.me/building-a-dead-simple-word-recognition-engine-using-convnet-in-keras-25e72c19c12b)
- Speacker Recognition: training audio books, testing on youtube videos - [link](https://towardsdatascience.com/automatic-speaker-recognition-using-transfer-learning-6fab63e34e74)

### Others
- Annimation [link](https://nbviewer.jupyter.org/gist/joshfp/85d96f07aaa5f4d2c9eb47956ccdcc88/lesson2-sgd-in-action.ipynb)
- Genome - [link](https://medium.com/@alenaharley/tumor-normal-sequencing-is-this-variant-real-7d972df7242a) 
- Tumor classification - [link](https://medium.com/@alenaharley/tumor-classification-using-gene-expression-data-poking-at-a-problem-using-fast-ai-again-8633c2256c85)

### Snippets

Use existing callback for saving activations - or at least simplify code using [HooksCallback](http://docs.fast.ai/callbacks.hooks.html) Untested, but something like:
```
class StoreHook(HookCallback):
    def on_train_begin(self, **kwargs):
        super().on_train_begin(**kwargs)
        self.acts = []
    def hook(self, m, i, o): return o
    def on_batch_end(self, train, **kwargs): self.acts.append(self.hooks.stored)
```
Pass a list of modules to the ctor to hook whatever layers you like.

#### Timeseries
Several approaches can be tested, and the one that works better is one where time series are mapped to an image by means of a polar coordinate transformation called the Gramian Angular Field (GAF). This creates a rich graphical representation of a univariate time series. If you are interested you can read this [paper](https://aaai.org/ocs/index.php/WS/AAAIW15/paper/viewFile/10179/10251) (Encoding Time Series as Images for Visual Inspection and Classification Using Tiled Convolutional Neural Networks). Based on this the univariate time series on the left is transformed into the image on the right. [notebook](https://gist.github.com/oguiza/6b08fd42921e6b0de14e9ee2e8e0bfa7)
- pyts for timeseries transformation and classification - [documentation](https://johannfaouzi.github.io/pyts/index.html)

#### NLP 
- Legal Text Classifier with ULMFit - [dataset](https://www.singaporelawwatch.sg/Judgments) [poster](https://docs.google.com/presentation/d/1U9VkRz6nKTz9Vbtoe0UuGHONTkd7XzWZBGlY_ihNbjQ/edit#slide=id.g25df3f774b_0_170)

### Reinforcement Learning
- Get started on RL [link](https://spinningup.openai.com/en/latest/)

#### Neural Net Training
- Cyclical Learning Rates for Training Neural Networks [paper](https://arxiv.org/pdf/1506.01186.pdf)
- A disciplined approach to neural network hyper-parameters: Part 1 -- learning rate, batch size, momentum, and weight decay - [paper](https://arxiv.org/abs/1803.09820)

#### Resources
- Deep Learning Resources - [link](https://sgfin.github.io/learning-resources/)
- A collection of video resources for ML - [link](https://github.com/dustinvtran/ml-videos)
- Comprehensive view of principles of Encoder-decoder neural networks - [link](https://docs.wixstatic.com/ugd/a5aa2f_1f1dac65975b41489eb9a6ca663cbf1b.pdf)
