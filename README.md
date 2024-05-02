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