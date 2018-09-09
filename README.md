**University of Pennsylvania, CIS 565: GPU Programming and Architecture,
Project 1 - Flocking**

!(images/flocking.gif)

* Salaar Kohari
  * [LinkedIn](https://www.linkedin.com/in/salaarkohari), [personal website](http://salaar.kohari.com)
* Tested on: Windows 10, Intel Xeon @ 3.7GHz 32GB, GTX 1070 8GB (SIG Lab)

### Description

Implemented flocking behavior for thousands of particles on the GPU. Flocking behavior includes cohesion, separation, and alignment rules.

The naive implementation involves parallel computation of each particle checking all other particles and performing rules if they are within the neighborhood distance. This and other implementations require device side storage of position and (ping-pong) velocity buffers.

The grid implementation involves dividing the space into a 3D grid and only checking the particles in cells that may contain neighbors. A cohesive grid implementation does the same with additional memory alignment for better performance at higher particle count. Grid implementations require new data arrays to be stored and sorted in order to keep track of particles and their corresponding grid cell.

### Details/Analysis

For naive implementation, fps goes from 1940 (100/1,000 boids) to 565 (5,000 boids) dropping to 53 (20,000 boids). The time taken is directly related to the number of boids since it must check all boids on each thread to calculate flocking behavior.

!(images/naiveFPS.png)

Grid implementations were attempted, but the program kept crashing. With GPU debug not working on the lab computer, I was unable to figure out the problem.

I expect that boid count wouldn't have as drastic effect on the grid based implementation, since more boids would have to be checked but compute time would increase logarithmically (checking nearby cells) rather than linearly with boid count.

Coherent uniform grid should also speed up performance as the number of boids gets large (in the tens of thousands) due to faster memory access adding up. Changing cell width and cells to check would slow down performance because it would take longer to map the grid arrays as well as to search more cells.

Lastly, as a note, I changed the gpu architecture to sm_50 to support my GPU in CMakeLists.txt.