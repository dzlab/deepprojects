## Visualization
```python
import matplotlib.pyplot as plt
```
### Show a bunch of images
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

## NLP
### FastText's word2vec
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
### Install python library from a github repo
```bash
!pip install git+https://github.com/facebookresearch/fastText.git
```
