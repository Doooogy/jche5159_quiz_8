# jche5159_quiz_8

**Part 1-Imaging Technique Inspiration**

- Inspring images:  
Resouce: https://www.youtube.com/watch?v=QOcoO3KcPos
![Image 1](readmeImages/Mandelbrot%20Set_1.jpg)
*Screenshot 1 created by DawraMath*
![Image 2](readmeImages/Mandelbrot%20set_2.jpg)
*Screenshot 2 created by DawraMath*

- Thoughts：
  - Technique:  
    Based on the Mandelbrot principle, generating self-similar subsets.
  - Reason for selection:  
    A simulation of natural ecology, visually striking, full of random beauty and vitality.
  - How can it be beneficial:  
    To add a sense of surprise to the outcome, it can create a state of vigorous growth that help me convey my concept of life.
  
------------------------------------
**Part 2-Coding Technique Exploration**

- Coding Technique:  
  Fractal Trees-Space Colonization
- Comment:  
  By generating hierarchical structures, new branches with self-similarity can be formed, creating random images resembling "tree" shapes.
- Sketch:
![Image 3](readmeImages/Image%203.jpg)
*Process*
![Image 4](readmeImages/Image%204.jpg)
*Outcome*
- Code Link:https://github.com/CodingTrain/Coding-Challenges/tree/main/017_SpaceColonizer/Processing
  - branch.js
```
  function Branch(parent, pos, dir) {
  this.pos = pos;
  this.parent = parent;
  this.dir = dir;
  this.origDir = this.dir.copy();
  this.count = 0;
  this.len = 5;

  this.reset = function() {
    this.dir = this.origDir.copy();
    this.count = 0;
  }


  this.next = function() {
    var nextDir = p5.Vector.mult(this.dir, this.len);
    var nextPos = p5.Vector.add(this.pos, nextDir);
    var nextBranch = new Branch(this, nextPos, this.dir.copy());
    return nextBranch;
  }

  this.show = function() {
    if (parent != null) {
      stroke(255);
      strokeWeight(4);
      line(this.pos.x, this.pos.y, this.parent.pos.x, this.parent.pos.y);
    }

  }
}
```
  - leaf.js
```
function Leaf() {
  this.pos = createVector(random(width), random(height - 100));
  this.reached = false;

  this.show = function() {
    fill(255);
    noStroke();
    ellipse(this.pos.x, this.pos.y, 4, 4);
  }

}
```
  - sketch.js
```
var tree;
var max_dist = 100;
var min_dist = 10;

function setup() {
  createCanvas(640, 360);
  tree = new Tree();
}

function draw() {
  background(0);
  tree.show();
  tree.grow();
}
```
  - style.css
```
html, body {
  margin: 0;
  padding: 0;
}
canvas {
  display: block;
}
```
  - tree.js
```
function Tree() {
  this.leaves = [];
  this.branches = [];

  for (var i = 0; i < 1500; i++) {
    this.leaves.push(new Leaf());
  }
  var pos = createVector(width / 2, height);
  var dir = createVector(0, -1);
  var root = new Branch(null, pos, dir);
  this.branches.push(root);
  var current = root;
  var found = false;
  while (!found) {
    for (var i = 0; i < this.leaves.length; i++) {
      var d = p5.Vector.dist(current.pos, this.leaves[i].pos);
      if (d < max_dist) {
        found = true;
      }
    }
    if (!found) {
      var branch = current.next();
      current = branch;
      this.branches.push(current);
    }
  }

  this.grow = function() {
    for (var i = 0; i < this.leaves.length; i++) {
      var leaf = this.leaves[i];
      var closestBranch = null;
      var record = max_dist;
      for (var j = 0; j < this.branches.length; j++) {
        var branch = this.branches[j];
        var d = p5.Vector.dist(leaf.pos, branch.pos);
        if (d < min_dist) {
          leaf.reached = true;
          closestBranch = null;
          break;
        } else if (d < record) {
          closestBranch = branch;
          record = d;
        }
      }

      if (closestBranch != null) {
        var newDir = p5.Vector.sub(leaf.pos, closestBranch.pos);
        newDir.normalize();
        closestBranch.dir.add(newDir);
        closestBranch.count++;
      }
    }

    for (var i = this.leaves.length - 1; i >= 0; i--) {
      if (this.leaves[i].reached) {
        this.leaves.splice(i, 1);
      }
    }

    for (var i = this.branches.length - 1; i >= 0; i--) {
      var branch = this.branches[i];
      if (branch.count > 0) {
        branch.dir.div(branch.count + 1);
        this.branches.push(branch.next());
        branch.reset();
      }
    }
  }





  this.show = function() {
    for (var i = 0; i < this.leaves.length; i++) {
      this.leaves[i].show();
    }

    for (var i = 0; i < this.branches.length; i++) {
      this.branches[i].show();
    }

  }

}
```
  - index.html
```
<!DOCTYPE html>
<html>

<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.3/p5.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.3/addons/p5.dom.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.3/addons/p5.sound.min.js"></script>
  <link rel="stylesheet" type="text/css" href="style.css">
  <meta charset="utf-8" />

</head>

<body>
  <script src="sketch.js"></script>
  <script src="leaf.js"></script>
  <script src="branch.js"></script>
  <script src="tree.js"></script>
</body>

</html>
```