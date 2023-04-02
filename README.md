# grid-based-fastSLAM
Key* Grid-based Fast SLAM

[image0]: img/a1.jpg "img0"
[image1]: img/a1.png "img1"
[image2]: img/a2.png "img2"
[image3]: img/a3.png "img3"
[image4]: img/a4.png "img4"
[image5]: img/a5.png "img5"
[image6]: img/a6.jpeg "img6"
[image7]: img/a7.png "img7"
[image8]: img/a8.png "img8"

# Forms SLAM
![alt text][image0]


## Online SLAM
![alt text][image1]
Probability equation: $p(x_{t} , m | z_{1:t} , u_{1:t})$

Thee posterior represented by the instantaneous pose $x_{t}$ and the map m given the measurements $z_{1:t}$ and controls $u_{1:t}$. In online SLAM, estimate the current pose and update the map give the current measurements and controls.

$x_{t}$ : the instantaneous pose

m : map

$z_{1:t}$ : measurements

$u_{1:t}$ : controls


## Full SLAM (Offline SLAM0
Probability equation: $p(x_{1:t} , m | z_{1:t} , u_{1:t})$

Thee posterior represented by the robot's trajectory $x_{t}$ and the map m given the measurements $z_{1:t}$ and controls $u_{1:t}$. In Full SLAM, estimate the entire trajectory and update the map give the all the measurements and controls.

$x_{t}$ : the instantaneous pose

m : map

$z_{1:t}$ : measurements

$u_{1:t}$ : controls

![alt text][image2]

# Nature of SLAM
![alt text][image3]

## Continuous
A robot continuously collects odometry information to estimate the robot poses and continuously senses the environment to estimate the location of the object or landmark. Thus, both robots poses and object location are continuous aspects of the SLAM problem.

## Discrete
robots continuously sense the environment to estimate the location of the objects, when doing so SLAM algorithms have to identify if a relation exists between any newly detected objects and previously detected ones. This helps the robot understand if it has been in this same location before. At each moment, the robot has to answer the question, “Have I been here before?”. The answer to this question is binary - either yes or no - and that’s what makes the relation between objects a discrete component of the SLAM problem.This discrete relation between objects is known by correspondence.

![alt text][image4]

### Correspondence

The FastSLAM algorithm solves the Full SLAM problem with known correspondences.

`Estimating the Trajectory`: FastSLAM estimates a posterior over the trajectory using a particle filter approach. This will give an advantage to SLAM to solve the problem of mapping with known poses.

`Estimating the Map`: FastSLAM uses a low dimensional Extended Kalman Filter to solve independent features of the map which are modeled with local Gaussian. The custom approach of representing the posterior with particle filter and Gaussian is known by the Rao-Blackwellized particle filter approach.

With the MCL, FastSLAM estimates the robot trajectory. With the Low-Dimensional EKF, FastSLAM estimates features of the map.


## Fast SLAM
The main advantage of the FastSLAM algorithm is that it uses a particle filter approach to solve the SLAM problem. Each particle will hold a guess of the robot trajectory, and by doing so, the SLAM problem is reduced to mapping with known poses.
![alt text][image5]

### `FastSLAM 1.0`
It is simple and easy to implement. However, this algorithm is known to be inefficient since particle filters generate sample inefficiency.

### `FastSLAM 2.0`
It overcomes the inefficiency of FastSLAM 1.0 by imposing a different distribution, which results in a low number of particles. Keep in mind that both of the FastSLAM 1.0 and 2.0 algorithms use a low dimensional Extended Kalman filter to estimate the posterior over the map features.

Both FastSLAM 1.0 and 2.0 algorithm presents a big disadvantage since it must always assume that there are known landmark positions, and thus with FastSLAM we are not able to model an arbitrary environment

### `Grid-based FastSLAM`
![alt text][image6]
The third instance of FastSLAM is really an extension to FastSLAM known as the grid-based FastSLAM algorithm, which adapts FastSLAM to grid maps. The grid mapping algorithm you can model the environment using grid maps without predefining any landmark position. The Grid-based FastSLAM algorithm estimates the robot trajectory using the MCL. Then, the Grid-based FastSLAM algorithm estimates the map by assuming known poses and using the occupancy grid mapping algorithm.

### Grid-based FastSLAM Techniques
![alt text][image7]
`Sampling Motion` $p(x_{t} | x_{t-1}^{[k]} , u_{t})$: Estimates the current pose given the k-th particle previous pose and the current controls u.

`Map Estimation` $p(m_{t} | z_{t}, x_{t}^{[k]} , m_{t-1}^{[k]})$: Estimates the current map given the current measurements, the current k-th particle pose, and the previous k-th particle map

`Importance Weight` $p(z_{t} | x_{t}^{[k]} , m^{[k]})$: Estimates the current likelihood of the measurement given the current k-th particle pose and the current k-th particle map.

The sampling motion and importance weight will be both solved with the MCL algorithm whereas the map estimation technique will be solved with the occupancy grid mapping algorithm.

Algorithm:
![alt text][image8]
