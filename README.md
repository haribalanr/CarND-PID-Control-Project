# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program
------
Overview:

This work was performed as part of the Udacity Self-Driving car Engineer nano degree program. Project work involved building a PID controller and tune its hyperparameters by applying the general processing flow and test it against the simulator. The simulator provides cross-track error (CTE), speed, and steering angle data via local websocket. The PID program responds with steering and throttle commands to drive the car reliably around the simulator track without crashing.

PID/Proportional Integral Differential.

Tuning Hyper Parameters:

I first started playing with hyperparameters manually, and took reference from the course work as well. I also employed the processo of Twiddle, where we programmatically, tune the hyperparameter on a loop based on the feedback (dp). However, initial runs directly using twiddle process did not succeed and resulted in car leaving truck and crash. After bunch of manual tries, (and some twiddle) was able to get the car around the track reliably meaning arriving at a resonable hyperparams. Later, I employed twiddle interatively to get better results. I felt it necessary to complete a full lap with each change in parameter because it was the only way to get a decent "score" (total error) for the parameter set. For this reason my parameter changes are allowed to set in for 100 steps and are then evaluated for 2000. I allowed Twiddle to continue for over 1 million steps (or roughly 400-500 trips around the track) to fine tune the parameters to their final values (P: 0.1346, I: 0.0002707, D: 3.053).

I  have also implemented a PID controller for the throttle, to maximize the car's speed around the track. The throttle PID controller is fed the magnitude of the CTE to avoid throttle up for right-side CTE and down for left-side CTE. For this reason the throttle controller doesn't include an I component (set to 0), which would only grow indefinitely. The throttle controller was also fine-tuned using the same Twiddle loop, simultaneously with the steering controller. 

P (proportional) component had the best directly observable effect on the car's behavior. It causes the car to steer proportional (and opposite) to the car's distance from the center of the lane (= CTE) - if the car is far to the right it steers hard to the left, if it is slightly to the left it steers slightly to the right.

D (differential) component counteracts the P component's tendency to oscillate and overshoot the center line. A properly tuned D parameter will cause the car to approach the center line smoothly without too much oscillation/wobbly effect.

I (integral) component counteracts a bias in the CTE which prevents the P-D controller from reaching the center line. This bias can take several forms, such as a steering drift, however for this project work this component particularly serves to reduce the CTE around curves.

Here is the video with PID:

Here is video with PD: (A very slight difference observed)


-------

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
