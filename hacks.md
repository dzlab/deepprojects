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

## Visualization
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
