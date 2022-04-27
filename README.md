<h1 align="center">
  CLASSIX :honeybee:

</h1>

<h3 align="center">
  <strong> Fast and explainable clustering based on sorting </strong>  
</h3>

[![Publish](https://github.com/nla-group/classix/actions/workflows/package_release.yml/badge.svg?branch=master)](https://github.com/nla-group/classix/actions/workflows/package_release.yml)
[![License: MIT](https://anaconda.org/conda-forge/classixclustering/badges/license.svg)](https://github.com/nla-group/classix/blob/master/LICENSE)
[![codecov](https://codecov.io/gh/nla-group/classix/branch/master/graph/badge.svg?token=D4MQZS67H1)](https://codecov.io/gh/nla-group/classix)
[![!pypi](https://img.shields.io/pypi/v/ClassixClustering?color=orange)](https://pypi.org/project/ClassixClustering/)
[![Download Status](https://static.pepy.tech/badge/ClassixClustering)](https://pypi.org/project/ClassixClustering/)
[![Documentation Status](https://readthedocs.org/projects/classix/badge/?version=latest)](https://classix.readthedocs.io/en/latest/?badge=latest)
[![Anaconda-Server Badge](https://anaconda.org/conda-forge/classixclustering/badges/version.svg)](https://anaconda.org/conda-forge/classixclustering)
[![Anaconda-Server Badge](https://anaconda.org/conda-forge/classixclustering/badges/platforms.svg)](https://anaconda.org/conda-forge/classixclustering)
[![Anaconda-Server Badge](https://anaconda.org/conda-forge/classixclustering/badges/latest_release_date.svg)](https://anaconda.org/conda-forge/classixclustering)
[![Anaconda-Server Badge](https://anaconda.org/conda-forge/classixclustering/badges/installer/conda.svg)](https://anaconda.org/conda-forge/classixclustering)
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nla-group/classix/HEAD)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.6329792.svg)](https://doi.org/10.5281/zenodo.6329792)
<!---[![PyPI pyversions](https://img.shields.io/pypi/pyversions/ClassixClustering.svg)](https://pypi.python.org/pypi/ClassixClustering/)-->

## :sparkles: Features

CLASSIX is a fast and explainable clustering algorithm based on sorting. Here are a few highlights:

- Ability to cluster low and high-dimensional data of arbitrary shape efficiently.
- Ability to detect and deal with outliers in the data.
- Ability to provide textual explanations for the generated clusters.
- Full reproducibility of all tests in the accompanying paper.
- Support of Cython compilation.

``CLASSIX`` is a contrived acronym of *CLustering by Aggregation with Sorting-based Indexing* and the letter *X* for *explainability*. CLASSIX clustering consists of two phases, namely a greedy aggregation phase of the sorted data into groups of nearby data points, followed by a merging phase of groups into clusters. The algorithm is controlled by two parameters, namely the distance parameter ``radius`` for the group aggregation and a ``minPts`` parameter controlling the minimal cluster size. 

**Here is a video abstract of CLASSIX:** 

[<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/classix_video_screenshot.png width=600 />](https://www.youtube.com/watch?v=K94zgRjFEYo)
 
A detailed documentation (work in progress), including tutorials, is available at [![Dev](https://img.shields.io/badge/docs-latest-blue.svg)](https://classix.readthedocs.io/en/latest/). 

## :rocket: Install

CLASSIX has the following dependencies:

- cython (recommend >= 0.27)
- numpy >=1.3.0 (recommend >= 1.20.0)
- scipy >= 1.2.1
- requests
- matplotlib
- pandas

### pip (recommended)
To install the current CLASSIX release via PIP use:

```pip install classixclustering```

To check the CLASSIX installation use:

```python -m pip show classixclustering```

### conda

To install CLASSIX using conda use:

```conda install -c conda-forge classixclustering```

To show the installed version use:

```conda list classixclustering```

### Download

Download this repository via:

```git clone https://github.com/nla-group/classix.git```

##  :checkered_flag: Quick start

Here is an example clustering a synthetic dataset: 

```Python
from sklearn import datasets
from classix import CLASSIX

# Generate synthetic data
X, y = datasets.make_blobs(n_samples=1000, centers=2, n_features=2, random_state=1)

# Call CLASSIX
clx = CLASSIX(radius=0.5, verbose=0)
clx.fit(X)
```

The cluster labels of each data point are available in ``clx.labels_``:
```Python
import matplotlib.pyplot as plt
plt.figure(figsize=(10,10))
plt.scatter(X[:,0], X[:,1], c=clx.labels_)
plt.show()
```

## :mortar_board: The explain method

#### Example 1
CLASSIX provides an API for the easy visualization of clusters and to explain the assignment of data points to their clusters. To get an overview of the data, the location of starting points, and their associated groups, type:

```Python
clx.explain(plot=True)
```
<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/explain_viz.png width=500 />

The starting points, shown as the small red boxes, can be thought of as a coarse representation of the data. Each starting point is associated with a group of data points, and groups are merged into clusters. The `explain` method returns a textual summary of the clustering:

```
A clustering of 5000 data points with 2 features has been performed. 
The radius parameter was set to 0.50 and MinPts was set to 0. 
As the provided data has been scaled by a factor of 1/6.01,
data points within a radius of R=0.50*6.01=3.01 were aggregated into groups. 
In total 7903 comparisons were required (1.58 comparisons per data point). 
This resulted in 14 groups, each uniquely associated with a starting point. 
These 14 groups were subsequently merged into 2 clusters. 
A list of all starting points is shown below.
----------------------------------------
 Group  NrPts  Cluster  Coordinates 
   0     398      0     -1.19 -1.09 
   1    1073      0     -0.65 -1.15 
   2     553      0     -1.17 -0.56 
  ---      lines omitted        ---
  11       6      1       0.42 1.35 
  12       5      1       1.24 0.59 
  13       2      1        1.0 1.08 
----------------------------------------
In order to explain the clustering of individual data points, 
use .explain(ind1) or .explain(ind1, ind2) with indices of the data points. 
```

In the above table, *Group* denotes the group label, *NrPts* denotes the number of data points in the group, *Cluster* is the cluster label assigned to the group, and the final column shows the normalized *Coordinates* of the starting point. In order to explain the cluster assignment of a particular data point, simply provide its index to the explain method:

```Python
clx.explain(0, plot=True)
```
<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/None0.png width=720 />
Output:

```
The data point 0 is in group 2, which has been merged into cluster #0.
```

We can also query why two data points ended up in the same cluster, or not: 

```Python
clx.explain(0, 2000, plot=True)
```
<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/None0_2000.png width=720 />
Output:

```
The data point 0 is in group 2, which has been merged into cluster 0.
The data point 2000 is in group 10, which has been merged into cluster 1.
There is no path of overlapping groups between these clusters.
```

#### Example 2

CLASSIX also supports dataframes and using dataframe indices to refer to data points:

```Python
X, _ = make_blobs(n_samples=5, centers=2, n_features=2, cluster_std=1.5, random_state=1)
X = pd.DataFrame(X, index=['Anna', 'Bert', 'Carl', 'Tom', 'Bob'])

           0     |     1
Anna | -7.804551 | -7.043560
Bert | -9.519154 | -4.327404
Carl | -0.361448 |  0.954182
Tom  |  0.957658 |  3.264680
Bob  | -2.451818 |  2.797037


clx = CLASSIX(radius=0.5)
clx.fit_transform(X)
```

Output:
```
CLASSIX(sorting='pca', radius=0.5, minPts=0, group_merging='distance')
The 5 data points were aggregated into 4 groups.
In total 3 comparisons were required (0.60 comparisons per data point). 
The 4 groups were merged into 2 clusters with the following sizes: 
      * cluster 0 : 3
      * cluster 1 : 2
Try the .explain() method to explain the clustering.
```

Let's take a look at the data and how it has been clustered:
```Python
clx.explain(plot=True, figsize=(2,2), sp_fontsize=12)
```
<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/explain_viz_df.png width=180 />

As before, we can explain the clustering of individual data points:
```Python
clx.explain(index1='Tom', index2='Bert', plot=True, sp_fontsize=12)
```

Output:
<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/NoneTom_Bert.png width=600 />

```
----------------------------------------
 Group  NrPts  Cluster Coordinates  Label
   3      1       0      0.76 0.65    Tom
   1      1       1     -0.9 -0.55   Bert
----------------------------------------
The data point Tom is in group 3, which has been merged into cluster 0.
The data point Bert is in group 1, which has been merged into cluster 1.
There is no path of overlapping groups between these clusters.
```

## :raising_hand: Frequently Asked Questions

### How to tune the parameters `radius` and `minPts`?

Generally, we recommend first running CLASSIX with a relatively large `radius` parameter, such as `radius=1`, and `minPts=0`. It also helps to use `verbose=1` to get more detailed feedback from the method. Typically, the larger the `radius` parameter, the faster the method performs and the smaller the number of computed clusters. If the number of clusters is too small, successively reduce the `radius` parameter until a ``good`` (depending on context) number of meaningful clusters is obtained. If there are unwanted noise clusters containing just a small number of data points, increase the `minPts` parameter to remove them. If, for example, `minPts=14`, all clusters with fewer than 14 data points will be reassigned to larger clusters. 

Here is an example that demonstrates the effect of `minPts`:
```Python
from sklearn import datasets
from classix import CLASSIX
# generate synthetic data
X, y = datasets.make_blobs(n_samples=1000, centers=2, n_features=2, random_state=0)
# run CLASSIX
clx = CLASSIX(sorting='pca', radius=0.15, group_merging='density', verbose=1, minPts=14)
clx.fit(X)
# draw figure
plt.figure(figsize=(8,8))
plt.rcParams['axes.facecolor'] = 'white'
plt.scatter(X[:,0], X[:,1], c=clx.labels_)
plt.xticks([])
plt.yticks([])
plt.show()
```

As we can see, there are two core clusters and many smaller ones with 14 or less data points. By chosing `minPts=14` we can reassign these noise clusters to the core clusters. 
```
CLASSIX(sorting='pca', radius=0.15, minPts=14, group_merging='density')
The 1000 data points were aggregated into 212 groups.
In total 5675 comparisons were required (5.67 comparisons per data point). 
The 212 groups were merged into 41 clusters with the following sizes: 
      * cluster 0 : 454
      * cluster 1 : 440
      * cluster 2 : 13
      * cluster 3 : 10
      * cluster 4 : 7
    --- lines omitted ---
      * cluster 38 : 1
      * cluster 39 : 1
      * cluster 40 : 1
As MinPts is 14, the number of clusters has been further reduced to 3.
```

The figure below shows the clustering result with `post_alloc=False`:

<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/demo5.png  width=360 />

So the next step is how we process these outliers, we can either marked as independent color as the figure above shows (the labels for outliers denote -1) or allocate them to the nearby clusters (each outlier will be assigned a label). If we determine to allow the outliers exist, we can set `post_alloc=False`. Otherwise outliers will be reassigned by setting `post_alloc=True`. The performance of `post_alloc=True` is as below. The post-processing depends on the problem context. 

<img src=https://raw.githubusercontent.com/nla-group/classix/master/docs/source/images/demo5_post.png />

### The visualization is not clear, is it a bug?

No, it is not a bug. Sometimes the default setting for visualization may result in a blurry picture, e.g., the boxes of starting points are too large, which hides the data object's color. You can personalize your visualization by specifying parameters for the ``.explain`` method. For example, we may set ``sp_alpha`` smaller to get more transparency for the box of starting points or set ``sp_pad`` smaller to get the box smaller, even we can change the color of that by specifying ``sp_fcolor`` to a shallow color. For more detail, we refer users to the documentation. Also, you can set `cmap` (e.g., `'bmh'`), `cmin` and `cmax` to customize a color map for a decent plot.

## :art: Reproducibility 
All experiment in the paper referenced below are reproducible by running the code in the folder of ["exp"](https://github.com/nla-group/classix/tree/master/exp).
Before running, ensure the dependencies `scikit-learn` and `hdbscan` are installed, and compile the ``Quickshift++`` code ([Quickshift++: Provably Good Initializations for Sample-Based Mean Shift](https://github.com/google/quickshift)). After configuring all of these, run the commands below. 

```
cd exp
python3 run exp_main.py
```

All results will be stored on ["exp/results"](https://github.com/nla-group/classix/tree/master/exp/results). Please let us know if you have any questions.



## :hammer: Contribution
Any form of contribution is welcome. Please be free to post issues and pull requests if you want to assist in documentation or code. To contribute, please fork the project and pull a request for your changes. We will strive to work through any issues and requests and get your code merged into the main branch. For those contributors, we will acknowledge their names in the release notes. 


###  Star & Fork
[![Stargazers repo roster for @nla-group/classix](https://reporoster.com/stars/nla-group/classix)](https://github.com/nla-group/classix/stargazers)
[![Forkers repo roster for @nla-group/classix](https://reporoster.com/forks/nla-group/classix)](https://github.com/nla-group/classix/network/members)

## :paperclip: Citation 

```bibtex
@techreport{CG22b,
  title   = {Fast and explainable clustering based on sorting},
  author  = {Chen, Xinye and G\"{u}ttel, Stefan},
  year    = {2022},
  number  = {arXiv:2202.01456},
  pages   = {25},
  institution = {The University of Manchester},
  address = {UK},
  type    = {arXiv EPrint},
  url     = {https://arxiv.org/abs/2202.01456}
}
```


## :memo: License
This project is licensed under the terms of the [MIT license](https://github.com/nla-group/classix/blob/master/LICENSE).


<p align="left">
  <a>
    <img alt="CLASSIX" src="https://raw.githubusercontent.com/nla-group/classix/master/docs/_images/nla_group.png" width="240" />
  </a>
</p>
