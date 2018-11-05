### Playing with activation layers
- Applying PCA to last layer before predictions - [notebook](https://rensdimmendaal.com/posts/feature-interpretation-for-breed-classifier/) 
- t-SNE to see how different are the classes - [notebook](https://github.com/kheyer/ML-DL-Projects/blob/master/Pets%20TSNE/pets_tsne.ipynb)
- Visualize different network layer activations - [notebook](https://github.com/MicPie/fastai_course_v3/blob/master/L1-stonefly_activations.ipynb)
- DeepDream - [notebook](https://github.com/kheyer/ML-DL-Projects/blob/master/Pytorch%20Deep%20Dream/dreaming.ipynb)

### Segmentation
- Full pipeline demo: poly -> pixels -> ML -> poly - [link](https://www.kaggle.com/lopuhin/full-pipeline-demo-poly-pixels-ml-poly)

### Others
- Annimation [link](https://nbviewer.jupyter.org/gist/joshfp/85d96f07aaa5f4d2c9eb47956ccdcc88/lesson2-sgd-in-action.ipynb)

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
