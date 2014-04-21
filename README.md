# Clusterfck
A js [cluster analysis](http://en.wikipedia.org/wiki/Cluster_analysis) library. Includes [Hierarchical (agglomerative) clustering](http://en.wikipedia.org/wiki/Hierarchical_clustering) and [K-means clustering](http://en.wikipedia.org/wiki/K-means_clustering). [Demo here](http://harthur.github.com/clusterfck/demos/colors/).

# Install

For node.js:

```bash
npm install clusterfck
```
Or grab the [browser file](http://harthur.github.com/clusterfck/demos/colors/clusterfck.js)


# K-means

```javascript
var clusterfck = require("clusterfck");

var colors = [
   [20, 20, 80],
   [22, 22, 90],
   [250, 255, 253],
   [0, 30, 70],
   [200, 0, 23],
   [100, 54, 100],
   [255, 13, 8]
];

// Calculate clusters.
var result = clusterfck.kmeans(colors, 3);

// Calculate cluster index for a new data point.
var clusterIndex = clusterfck.kmeans.classify([0, 0, 225]);
```

The second argument to `kmeans` is the number of clusters you want (default is `Math.sqrt(n/2)` where `n` is the number of vectors). It returns an object containing an array of the clusters and an array of the centroids, for this example:

```javascript
{ "clusters":
[
  [[200,0,23], [255,13,8]],
  [[20,20,80], [22,22,90], [0,30,70], [100,54,100]],
  [[250,255,253]]
],
 "centroids": [ ... ]
}
```

# Serialization

The toJSON() and fromJSON() methods are available for serialization.

```javascript
// Serialize centroids to JSON.
var json = clusterfck.kmeans.toJSON();

// Deserialize centroids from JSON.
var centroids = clusterfck.kmeans.fromJSON(json);

// Calculate cluster index from a previously serialized set of centroids.
var clusterIndex = clusterfck.kmeans.classify([0, 0, 225]);
```

# Hierarchical

```javascript
var clusterfck = require("clusterfck");

var colors = [
   [20, 20, 80],
   [22, 22, 90],
   [250, 255, 253],
   [100, 54, 255]
];

var clusters = clusterfck.hcluster(colors);
```

`hcluster` returns an object that represents the hierarchy of the clusters with `left` and `right` subtrees. The leaf clusters have a `value` property which is the vector from the data set.

```javascript
{
   "left": {
      "left": {
         "left": {
            "value": [22, 22, 90]
         },
         "right": {
            "value": [20, 20, 80]
         },
      },
      "right": {
         "value": [100, 54, 255]
      },
   },
   "right": {
      "value": [250, 255, 253]
   }
}
```

#### Distance metric and linkage

Specify the distance metric, one of `"euclidean"` (default), `"manhattan"`, and `"max"`. The linkage criterion is the third argument, one of `"average"` (default), `"single"`, and `"complete"`.

```javascript
var tree = clusterfck.hcluster(colors, "euclidean", "single");
```
