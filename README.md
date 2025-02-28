# fmiSWAP

This repository contains a watertank with model swap and a `Dockerfile` to build a running example using the Maestro co-orchestration engine for FMI-based co-simulation with the model swap feature including a FaultInject extension.

# Getting started - Quick setup

In order to quickly run the experiment and produce the results of the artefact paper ("Dynamic Runtime Integration of New Models in Digital Twins" [H. Ejersbo, K. Lausdahl, M. Frasheri, L. Esterle]) do the following:

## Clone this repo

Clone this repo locally

```bash
$ git clone https://github.com/lausdahl/SEAMS2023Artefact-fmiSWAP.git
```

Change to the repo directory before going to the next step.

## Build the image

```bash
$ docker build . --tag lausdahl/maestro:2.3.0-model-swap
```

## Run the example

```bash
$ docker run -it -v ${PWD}:/work/model/post  lausdahl/maestro:2.3.0-model-swap
```

After this step completes, you should see in the containing folder two files ```result.png``` and ```result.pdf```, showing the plot included in the paper. 

# DESCRIPTION

LeadCar: an FMU that implements both the leader car's dynamics and the acceleration function that it follows. Outputs its speed position and acceleration.
SimpleCar: an FMU that implements the follower car's dynamics, takes as input the desired acceleration and outputs its speed and position.
CACC: is an FMU that implements the Cooperative Adaptive Cruise Control algorithm, that takes as input speed position and eventually acceleration of the leader and outputs the desired acceleration that the simple car should have to follow the leader.
NewController: it's a neural network that takes the same inputs of the CACC and outputs the predicted desired acceleration and the loss function. It trains during the simulation and we would like, after the loss goes below a certain amount, to replace the CACC with it.

# Notes:

We cannot run the simulations with the swap fmi tool, but they work if ran with the INTO-CPS Application GUI (without swap fmi). The issue seems to be with the simulation part that runs after the transition.
There is also a found issue with the latests maestro version were if you run a simulation with a large enough end time (for example 150 seconds, start time set to 0 and step size set to 0.1), the simulation doesn't run at all.
