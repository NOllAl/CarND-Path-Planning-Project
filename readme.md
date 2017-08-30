# Description of line generation

The path generation follows very closely the outline video.

The general idea is that we have a set of "anchor points" (line 334-336): these are points, widely spaced apart, that 
serve as some fixed points that the vehicle is supposed to pass through. Once we have these points, we use a spline to 
smoothly interpolate between these anchor points (lines 357-370). Since the car cannot follow the entire path generated 
in the time between two code iterations, we first add the points from the previous code run and only concatenate with
the points from the spline. Let us note that all of this is done in *vehicle coordinates* (lines 347-354).

Next, it is necessary to ensure that the car does not accelarate or brake too fast: for this, we calculate a reference velocity 
(lines 286-290): if we are not too close to another car and are not too fast yet, we increase it by .3. This reference velocity 
is then used in lines 373-390 to ensure that the car is travelling at the desired velocity: there we fill up the target vectors
with the output from the spline ensuring that the car is going at the target velocity (line 381).

Up to this point, we have a description of a car that is able to drive within its own lane without regard of the other traffic.
The other cars are handled in lines 257-284: we loop over the sensor funsion data, which contains the information about the 
other cars on the road. We can read of the other Frenet coordinates of the other cars. First, we check if the car is in our 
lane, and if it is we check whether it is too close or not. If it is, we break and attempt a lane change.
