# Path Planning Project
## The Goal of this Project
In this project, your goal is to design a path planner that can create smooth, safe paths for the car to follow along a 3 lane highway with traffic. A successful path planner should be able to keep inside its lane, avoid hitting other cars and pass slower moving traffic all by using localization, sensor fusion, and map data.
## My Approach
### Speed management. 
In this part of the model, my goal was to achieve smooth acceleration up to the allowed maximum speed of 50 mph. At every timestep (0.02 sec), the algorithm increments the reference car speed used for estimating future waypoints by a small amount (0.224 m/s) so that the total jerk and acceleration are below allowed maximum. The increase of speed stops when the car’s speed is 49.5 mph or more. 
Additionally, in case of sensor fusion detects another car closer than 30 meters in the same lane in front of the ego, the algorithm would start to decrease the reference speed by 0.224 m / s. The speed is decreased until a full stop or until the slower object leaves the lane/increases the distance to more than 30 meters. 
The waypoints for the controllers are generated using C++ spline module that takes target lane, distance to be covered (based on the reference speed), the reference position of our car as inputs. Staying in the lane is helped by using the frenet coordinates.
### Safe lane change
For this projected I decided that my car should try to change lanes when there is a close obstacle in front and in its lane:
1) when the car encounter an obstacle closer than 30 meters ahead in its lane, it would try to change to the left lane if it is present and safe,
2) if not then it consider changing right (same conditions),
3) alternatively is stays in its lane (speed management would be decreasing the speed in this case).
The model continually checks the safety of the adjacent lanes. I used sensor fusion data about location and speed of other vehicles relative to the ego car. If there is a car in the considered lane that meets following conditions the change lane maneuvers was prohibited:
1) within 10 meters in front or behind my car
2) within 30 meters range which might enter 10-meter close zone within the next 3 seconds
On the technical side, I had some issues with sensor fusion frenet data. The s values were not aligned with the ego car’s data, so I had to approximate other car s values for XY coordinates and speed vectors.
### Conclusions
In my experiments, the model demonstrated sound results that meet the requirements. However, I have noticed that there is at least one type of collisions that it cannot avoid – aggressive lane change by another car. In such rear cases, it would make sense to introduce extreme behavior, for example, emergency braking or evasive maneuver, which would violate max acceleration/jerk criteria but still should be a preferred plan of actions.
