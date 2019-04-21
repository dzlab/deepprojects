## python
#### Debugging
You can use python debugger `pdb` to step through code.
```python
import pdb
pdb.set_trace()
```
- `pdb.set_trace()` to set a breakpoint
- `%debug` magic to trace an error (type this after an exception had occured)

Debugger commands you need to know:

| Command        | Short version           | Comment  |
| ------------- |:-------------:| -----:|
| `help` | `h`| for help |
| `step` | `s`| to step inside the current instruction. |
| `next` | `n`| simply type enter to run next instruction. |
| `continue`| `c`| to continue until next breakpoint. |
| `up`   | `u`| to change the context of the debugger and see what the previous call on stack, i.e. what called the current instruction.|
| `down` | `d`| (after a `u`) to go down agin and return to the previous debugger context.|
| `print`| `p`| to print a variable, e.g. `p my_variable`.|
| `list` | `l`| for listing the code context, i.e. lines before and after current instruction.|

More debugging tips can be found [here](https://www.digitalocean.com/community/tutorials/how-to-use-the-python-debugger).

#### Free Memory
```python
import gc
gc.collect()
```

#### Do stuff in parallel
```python
import tqdm
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

def parallel(func, arr, max_workers=None):
    "Call `func` on every element of `arr` in parallel using `max_workers`."
    if max_workers<2:
        _ = [func(o,i) for i,o in enumerate(arr)]
    else:
        with ProcessPoolExecutor(max_workers=max_workers) as ex:
            futures = [ex.submit(func,o,i) for i,o in enumerate(arr)]
            for f in tqdm(concurrent.futures.as_completed(futures), total=len(arr)): pass
```
Or using [joblib](https://joblib.readthedocs.io)
```python
from joblib import Parallel, delayed

def parallelize(func, iterator, n_jobs):
    Parallel(n_jobs=n_jobs)(delayed(func)(*item) for item in iterator)

parallelize(count_freqs, tasks, n_jobs)
```
#### Download urls into a folder
```python
import urllib
from tqdm import tqdm

def download(path, urls):
    for url in tqdm(urls):
        fname = url[url.rfind('/')+1:]
        urllib.request.urlretrieve(url, path/fname)
```
#### Check if an object has a specific attribute
```python
hasattr(obj, 'attribute_or_function_name')
```

#### __dunder__ thingies
To add specific behaviors to an object - [documentation](https://docs.python.org/3/reference/datamodel.html)
```python
def __getitem__(self, ...) # to get an item by index
def __getattr__(self, ...) # to get value of an attribute of this object
def __setattr__(self, ...) # to set value of an attribute of this object
def __del__(self, ...)     # to do something when the object is deleted (i.e. del(o))
def __init__(self, ...)    # to initialize the object
def __new__(self, ...)     # to do something when a new object instance is called
def __enter__(self, ...)   # to do something when the context of the object is entered (i.e. with o: )
def __exit__(self, ...)    # to do something when the context of the object is existed
def __len__(self, ...)     # to get len of the object (e.g. if it contains a collection)
def __add__(self, ...)     # to add support for + operation
def __call__(self, ...)    # to be able to call the object
def __repr__(self, ...)    # to modify the string representation
def __str__(self, ...)     # to add support for formatted print (i.e. str(o))
```

## Visualization (IPython and Jupyter)
```python
import matplotlib.pyplot as plt
```
#### Show a bunch of images
```python
def ceildiv(a, b):
    return -(-a // b)

def plots_from_files(imspaths, figsize=(10,5), rows=1, titles=None, maintitle=None):
    """Plot the images in a grid"""
    f = plt.figure(figsize=figsize)
    if maintitle is not None: plt.suptitle(maintitle, fontsize=10)
    for i in range(len(imspaths)):
        sp = f.add_subplot(rows, ceildiv(len(imspaths), rows), i+1)
        sp.axis('Off')
        if titles is not None: sp.set_title(titles[i], fontsize=16)
        img = plt.imread(imspaths[i])
        plt.imshow(img)
```

#### Disply source image along with the predition probabilities as bars
```
def plot_image(i, predictions_array, true_labels, images):
  predictions_array, true_label, img = predictions_array[i], true_labels[i], images[i]
  plt.grid(False)
  plt.xticks([])
  plt.yticks([])
  
  plt.imshow(img[...,0], cmap=plt.cm.binary)

  predicted_label = np.argmax(predictions_array)
  if predicted_label == true_label:
    color = 'blue'
  else:
    color = 'red'
  
  plt.xlabel("{} {:2.0f}% ({})".format(class_names[predicted_label],
                                100*np.max(predictions_array),
                                class_names[true_label]),
                                color=color)

def plot_value_array(i, predictions_array, true_label):
  predictions_array, true_label = predictions_array[i], true_label[i]
  plt.grid(False)
  plt.xticks([])
  plt.yticks([])
  thisplot = plt.bar(range(10), predictions_array, color="#777777")
  plt.ylim([0, 1]) 
  predicted_label = np.argmax(predictions_array)
 
  thisplot[predicted_label].set_color('red')
  thisplot[true_label].set_color('blue')


# Plot the first X test images, their predicted label, and the true label
# Color correct predictions in blue, incorrect predictions in red
num_rows = 5
num_cols = 3
num_images = num_rows*num_cols
plt.figure(figsize=(2*2*num_cols, 2*num_rows))
for i in range(num_images):
  plt.subplot(num_rows, 2*num_cols, 2*i+1)
  plot_image(i, predictions, test_labels, test_images)
  plt.subplot(num_rows, 2*num_cols, 2*i+2)
  plot_value_array(i, predictions, test_labels)
```

#### Rich display for NumPy arrays in the Jupyter Notebook
```bash
!pip install numpy_html
```
```python
import numpy_html
import numpy as np

np.set_printoptions(threshold=5, edgeitems=2)

np.arange(49).reshape(7, 7)
```

#### wildcard completions of attribute names, marked by using a ? at the end:
```python
> import altair as alt

> alt.*Transform?
alt.AggregateTransform
alt.BinTransform
alt.CalculateTransform
alt.FilterTransform
...
```

## NLP
#### FastText's word2vec
Download word2vec for the target language (e.g. [english](https://dl.fbaipublicfiles.com/fasttext/vectors-wiki/wiki.en.zip
))
```python
import fastText as ft
en_vecs = ft.load_model(str((PATH/'wiki.en.bin')))

def get_vecs(lang, ft_vecs):
    vecd = {w:ft_vecs.get_word_vector(w) for w in ft_vecs.get_words()}
    pickle.dump(vecd, open(PATH/f'wiki.{lang}.pkl','wb'))
    return vecd

en_vecd = get_vecs('en', en_vecs)
en_vecd = pickle.load(open(PATH/'wiki.en.pkl','rb'))
```

## Bash
#### Install python library from a github repo
```bash
!pip install git+https://github.com/facebookresearch/fastText.git
```
