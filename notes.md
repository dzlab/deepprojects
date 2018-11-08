### Playing with activation layers
- Applying PCA to last layer before predictions - [notebook](https://rensdimmendaal.com/posts/feature-interpretation-for-breed-classifier/) 
- t-SNE to see how different are the classes - [notebook](https://github.com/kheyer/ML-DL-Projects/blob/master/Pets%20TSNE/pets_tsne.ipynb)
- Visualize different network layer activations - [notebook](https://github.com/MicPie/fastai_course_v3/blob/master/L1-stonefly_activations.ipynb)
- DeepDream - [notebook](https://github.com/kheyer/ML-DL-Projects/blob/master/Pytorch%20Deep%20Dream/dreaming.ipynb)

### Segmentation
- Full pipeline demo: poly -> pixels -> ML -> poly - [link](https://www.kaggle.com/lopuhin/full-pipeline-demo-poly-pixels-ml-poly)

### Face Detection
- Using Dlib [python](https://engmrk.com/face-detection-application/) [javascript](https://medium.com/@muehler.v/node-js-face-recognition-js-simple-and-robust-face-recognition-using-deep-learning-ea5ba8e852)
0 Using Chrome FaceDetection API [sample](https://medium.com/@joomiguelcunha/lets-play-with-chrome-s-face-detection-api-ca13017a958f)

### Others
- Annimation [link](https://nbviewer.jupyter.org/gist/joshfp/85d96f07aaa5f4d2c9eb47956ccdcc88/lesson2-sgd-in-action.ipynb)
- Genome - [link](https://medium.com/@alenaharley/tumor-normal-sequencing-is-this-variant-real-7d972df7242a)

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

#### References
- Deep Learning Resources - [link](https://sgfin.github.io/learning-resources/)
