---
title: document-similarity-social-network
date: 2020-08-25 13:00:00 +08:00
tags: [Social Network, Natural Language Processing, Machine Learning]
description: This project is to compare the similarity of documents.
image: "/document-similarity-social-network/tf-idf.png"
---

# Document Similarity Social Network

## Flow

### Calculate tf-idf feature.

TF-IDF stands for “Term Frequency, Inverse Document Frequency.” It’s a way to score the importance of words (or “terms”) in a document based on how frequently they appear across multiple documents.

![tf-idf](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/document-similarity-social-network/tf-idf.png)

### Calculate cosine similarity matrix.

![similarity matrix](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/document-similarity-social-network/similarity_matrix.png)

### Calculate nodes relationships with threshold-based neighbors.

![pick neighbors](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/document-similarity-social-network/pick.gif)

## What is Vis.js

A dynamic, browser based visualization library. The library is designed to be easy to use, to handle large amounts of dynamic data, and to enable manipulation of and interaction with the data. The library consists of the components DataSet, Timeline, Network, Graph2d and Graph3d.

## Document Social Network Result

![document network result](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/document-similarity-social-network/document-network.png)

### Practical application

![document similarity application](https://raw.githubusercontent.com/ekko771/ekko771.github.io/master/_posts/document-similarity-social-network/recommand.png)

## Methodology references: 

* [The TF-IDF algorithm explained.](https://www.onely.com/blog/what-is-tf-idf)
* [Calculate cosine similarity matrix.](https://en.wikipedia.org/wiki/Cosine_similarity)
* Calculate nodes relationships with threshold-based neighbors.
## Tool references: 

* Social Network Visualization. 
    * [Vis.js](https://visjs.github.io/vis-network/examples/)
    * [[Network - groups](https://visjs.github.io/vis-network/docs/network/groups.html)
    * [[Network - nodes](https://visjs.github.io/vis-network/docs/network/nodes.html)
    * [[Network - edges](https://visjs.github.io/vis-network/docs/network/edges.html)
