# GraphWarData (Deprecated)
Note: This repo is **Deprecated** since GraphWar no longer uses these data.


# Usage of `.npz` datasets
```python
import os.path as osp
import numpy as np

def load_npz(filepath):
    filepath = osp.abspath(osp.expanduser(filepath))

    if not filepath.endswith('.npz'):
        filepath = filepath + '.npz'
    if osp.isfile(filepath):
        with np.load(filepath, allow_pickle=True) as loader:
            loader = dict(loader)
            for k, v in loader.items():
                if v.dtype.kind in {'O', 'U'}:
                    loader[k] = v.tolist()
            return loader
    else:
        raise ValueError(f"{filepath} doesn't exist.")
```
e.g., run load_npz('cora') and it returns a dict instance `loader`, it might have the following `keys`:
+ `adj_matrix`: scipy.sparse.csr_matrix, adjacency matrix. NOTE: the adjacency matrix might not be symmetric.
+ `attr_matrix`: scipy.sparse.csr_matrix or numpy.ndarray, node attribute matrix
+ `label`: scipy.sparse.csr_matrix or numpy.ndarray, node labels
+ `metadata` (if have): dict, additional metadata such as text.
