# El Grapho

<img width="300" src="https://raw.githubusercontent.com/ericdrowell/ElGrapho/master/img/elgrapho.png"/>

## What is El Grapho?

<img width="800" src="https://raw.githubusercontent.com/ericdrowell/ElGrapho/master/img/elgrapho-examples.png"/>

El Grapho is a high performance WebGL graph data visualization engine.  El Grapho can support millions of interactive nodes and edges in any modern browser.

https://www.elgrapho.com/

## Why Would I Use This?

If you need to build a graph visualization for the web of any kind, such as a tree, force directed graph, network graph, etc., and scale and performance are important to you, then El Grapho is a great option.  El Grapho was built to push the limits of modern browsers.

## Live Examples

* https://codepen.io/ericdrowell/pen/qLYrEm
* https://codepen.io/ericdrowell/pen/wNRyoZ

## Getting Started

To get started, you can install El Grapho from npm like this

```npm install elgrapho```

or you can download the latest El Grapho distribution file found here 

https://github.com/ericdrowell/ElGrapho/blob/master/engine/dist/ElGrapho.min.js

## API

To create a simple El Grapho data visualization, you can instantiate a new graph like this

```
let graph = new ElGrapho({
  container: document.getElementById('container'),
  model: {
    nodes: [
      {x: 0,    y: 0.6,  group: 0, label: 0},
      {x: -0.4, y: 0,    group: 1, label: 1},
      {x: 0.4,  y: 0,    group: 1, label: 2},
      {x: -0.6, y: -0.6, group: 2, label: 3},
      {x: -0.2, y: -0.6, group: 2, label: 4},
      {x: 0.2,  y: -0.6, group: 2, label: 5},
      {x: 0.6,  y: -0.6, group: 2, label: 6}
    ],
    edges: [
      {from: 0, to: 1},
      {from: 0, to: 2},
      {from: 1, to: 3},
      {from: 1, to: 4},
      {from: 2, to: 5},
      {from: 2, to: 6}
    ],
    width: 500,
    height: 500
  }
});
```

* ```container``` - DOM element that will contain the El Grapho graph.

* ```model.nodes``` - object that defines the nodes in the graph.  Each node is defined by a position (x and y), and also a group.  El Grapho x and y ranges are between -1 and 1.  If x is -1, then the node position is on the very left of the viewport.  If x is 0 it is in the center, and if x is 1 it is on the very right of the viewport.  Groups are integer values, and El Grapho automatically assigns colors to each unique grouping.  To add labels, simply add a ```label``` key and value pair to each node.

* ```model.edges``` - object that defines the edges between nodes based on their indices.  Each edge is defined by a from-node-index and a to-node-index.  In the example above, the first edge begins at node ```0``` and ends at node ```1```.  For non directed graphs, or bi-directional graphs, ```from``` and ```to``` are interchangeable. 

* ```model.width``` - number that defines the width of the El Grapho viewport in pixels.

* ```model.height``` - number defines the height of the El Grapho viewport in pixels.

* ```animations``` - boolean that defines animation strategy.  When animations is true, zoom and pan transitions will be animated.  Otherwise the transitions will be immediate.  Although animations utilize requestAnimationFrame for dynamic frame rates, in some situations you may prefer to set animations to false to improve transition performance for very high cardinality graphs with millions of nodes and edges.  The default is true.

* ```debug``` - boolean that can be used to enable debug mode.  Debug mode will show the node and edge count in the bottom right corner of the visualization.  The default is false.

* ```arrows``` - boolean that enables or disables edge arrows. The default is false.  For non directed or bi-directional graphs, you should keep ```arrows``` as ```false```.

### Models

Determining the positions of the nodes for your graph can be alot of work!  While it's nice to have the power to construct custom graph layouts, most El Grapho users will want to leverage the provided El Grapho models which will generate node positions for you.  Currently, ElGrapho supports ```ForceDirected```, ```Tree```, ```Web```, ```Cluster```, and ```Ring```

#### ForceDirected Model

```
let model = {
  nodes: [
    {group: 0},
    {group: 1},
    {group: 1},
    {group: 2},
    {group: 2},
    {group: 3}
  ],
  edges: [
    {from: 0, to: 1},
    {from: 0, to: 2},
    {from: 1, to: 3},
    {from: 1, to: 4},
    {from: 2, to: 5},
  ],
  width: 500,
  height: 500,
  steps: 30
};

graph = new ElGrapho({
  container: document.getElementById('container'),
  model: ElGrapho.models.ForceDirected(model)
});
```

The ```ForceDirected``` model uses Webcola to reduce edge crossing and optimize node spacing.  Be warned that force directed graphs take O(n^2) time, which means they may not be appropriate for generating models on the client with lots of nodes.  If it's possible to build your models in advance, it's a good idea to build the force directed graph model on the server and then cache it.  If you require your models to be constructed at execution time, and the number of nodes is very high, you may consider using an O(n) model substitude such as ```Web``` or ```Cluster```

The ```ForceDirected``` model accepts a ```steps``` property from the model config which can be used to define the accuracy of the result.  This is because force directed graphs require multiple passes to obtain the final result.  The default is 30.  Lowering this number will result in faster model construction but less accurate results.  Increasing this number will result in slower model construction but more accurate results. 

#### Tree Model

```
let model = {
  nodes: [
    {group: 0},
    {group: 1},
    {group: 1},
    {group: 2},
    {group: 2},
    {group: 3},
    {group: 3}
  ],
  edges: [
    {from: 0, to: 1},
    {from: 0, to: 2},
    {from: 1, to: 3},
    {from: 1, to: 4},
    {from: 2, to: 5},
    {from: 2, to: 6}
  ],
  width: 500,
  height: 500
};

let graph = new ElGrapho({
  container: document.getElementById('container'),
  model: ElGrapho.models.Tree(model),
  arrows: true
});
```

#### Web Model

```
let model = {
  nodes: [
    {group: 0},
    {group: 1},
    {group: 1},
    {group: 2},
    {group: 2},
    {group: 3},
    {group: 3}
  ],
  edges: [
    {from: 0, to: 1},
    {from: 0, to: 2},
    {from: 1, to: 3},
    {from: 1, to: 4},
    {from: 2, to: 5},
    {from: 2, to: 6}
  ],
  width: 500,
  height: 500
};

let graph = new ElGrapho({
  container: document.getElementById('container'),
  model: ElGrapho.models.Web(model),
  arrows: true
});
```

Web begin by positioning all of the nodes in a ring, and then iteratively move nodes closer together that share an edge.  This essentially moves nodes with lots of edges towards the center, and pushes nodes with few or no edges to the outside.  Often times, Web models looks fairly similar to ForceDirected models, but Web models run in O(n) time compared to ForceDirected models, which run in O(n^2) time, which makes them great alternatives for large numbers of nodes and edges.

#### Cluster Model

```
let model = {
  nodes: [
    {group: 0},
    {group: 1},
    {group: 1},
    {group: 2},
    {group: 2},
    {group: 3},
    {group: 3}
  ],
  edges: [
    {from: 0, to: 1},
    {from: 0, to: 2},
    {from: 1, to: 3},
    {from: 1, to: 4},
    {from: 2, to: 5},
    {from: 2, to: 6}
  ],
  width: 500,
  height: 500
};

let graph = new ElGrapho({
  container: document.getElementById('container'),
  model: ElGrapho.models.Cluster(model),
  arrows: true
});
```

The Cluster model clusters nodes by group.  If a single group is used for all of the nodes, ElGrapho will generate a single centered cluster.  If there are several groups used, ElGrapho will render distinct clusters.  Because Cluster models can be generated in ```O(n)``` time, i.e. linear time, they are very fast to construct compared to other models such as force directed graphs which are polynomial in time.

#### Ring Model

```
let model = {
  nodes: [
    {group: 0},
    {group: 1},
    {group: 1},
    {group: 2},
    {group: 2},
    {group: 3},
    {group: 3}
  ],
  edges: [
    {from: 0, to: 1},
    {from: 0, to: 2},
    {from: 1, to: 3},
    {from: 1, to: 4},
    {from: 2, to: 5},
    {from: 2, to: 6}
  ],
  width: 500,
  height: 500
};

let graph = new ElGrapho({
  container: document.getElementById('container'),
  model: ElGrapho.models.Ring(model),
  arrows: true
});
```


## Model Polymorphism

You may have noticed that the model config schema is identical for all models.  As a result, El Grapho visualizations are polymorphic, meaning you can pass the same data structure into different models and get different graph layouts.  Pretty cool!

## Server Side Model Generation

Because the El Grapho models are fully decoupled from the rendering engine itself, they can be executed on the client or on the server, depending on your needs.  For really complex models, it may be best to build the model on the server, deliver the output over http to the browser, and pass it directly into the El Grapho config.

## Controls

El Grapho has controls in the upper right corner of the visualization that enable users to navigate large and complex graphs.  These controls appear when you mouseover the visualizztion.  There are three modes:

* __select__ - use this mode to select nodes
* __zoom__ - use this mode to draw zoom boxes around areas of interest or to zoom into a particular region of the graph
* __pan__ - use this mode to pan the visualization around

And there are three action buttons:

* __reset__
* __zoom in__
* __zoom out__

## Tooltips

El Grapho ships with a default template and default content.  It is assumed however that you will be providing your own tooltip content (at the end of the day, most people want something custom anyways).  To set the tooltip template, simply do the following:

```
let graph = new ElGrapho(config);

graph.tooltipTemplate = function(index, el) {
  el.innerHTML = 'node index: ' + index;
};
```

This means that your tooltips can be anything!  You can show lots of information about the node, insert images, etc.

## Events

El Grapho also has a built in event bus.  Currently, the supported events are:

* ```idle```
* ```node-mouseover```
* ```node-mouseout```
* ```node-click```

and you can use these events like this:

```
let graph = new ElGrapho(config);

graph.on('idle', function() {
  console.log('idle');
});

graph.on('node-mouseover', function(evt) {
  console.log('node-mouseover: ' + evt.dataIndex);
});
```

## About the Name and Logo

Why is this called El Grapho? - *Why not?*

Okay... and why is the logo a skeleton with a rose in his mouth? - *Why not?*





