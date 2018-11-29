# Parallel particle swarm optimization on a graphics processing unit with application to trajectory optimization Reflection

## Abstract

Aim is to reduce computational time by paralleizing PSO on GPU. Looking to compare a sequantial CPU implementation to using CUDA to target Nvidea architectures. (Not a fair comparison)
Apparently fitness evaluation, updating particle position and velocities all parallelized.
Invesitgate the impact of dimensions, number of particles and size of thread blocks on GPU.
Results show for paralleized GPU implementation, speedups over sequantial CPU implementation.

Well okay then, let's look at that in further detail.

## Introduction

* PSO is a optimization algorithm (typically for simulations with complex outcomes) which is considered an evolutionary algorithm.
* Inspired by social behaviour of bird flocking and fish schools.
* Fitness function for each.
* If there are a large number of particles, this means every iteration a large number of fitness evaluations must be made.
* Becomes even more complex if fitness functions highly computationally expensive.
* A nieve implmentation on CPU would evaluate each sequantially. When this is a great setup for parallelization, meeting the SIMD problem type. (Expand on why this problem suits paralleization, particularly SIMD and therefore GPU architecture)
* Well best idea really is to take advantage of CPU and GPU. Using each for parts of the problem they are best suited for.
* GPGPU becoming more recognized. Frameworks like CUDA becoming readily available and accessible to developers.
* Only a few research literatures carried out on PSO using GPU acceleration. And they usually just parallize the fitness evaluation, when other aspects can be too. Like updating positions and velocities. (Apparently meaning a lot of copy calls made between CPU and GPU?)
* Further, investigate the conrollable parameters effect on performance.

## Review of PSO

* PSO is a stochastic(meaning random probability involved) global optimization technique. 
* Each particle has a position + velocity. Using its own state, local states and a global bets state. The particle decides on its next best position.
* The best position is decided on using some fitness function.
* Whole problem space, represented by designed model space is explored by flock of particles. Moving based on best local particle and global flock best positions.
* Idea is to converge on global best point.

## Intro Graphics processing parallel computing

* CPUs reaching limit on clocks speed. Around 4GHz today. Idea to increase performance further by using more cores, threads.
* GPU architecture aimed to process with high concurrency with same instuctions on multiple sets of similar data.
* Better floating point calcualtions and memory bandwith(I believe for textures).
* Applied to general purpose computing problems like fluid dynamics and molecular mechanics.
* Copy data to GPU. Run GPU program on data. Copy data to Host.

## Graphics processing unit-based particle swarm optimization

* 

## Comparison Studies

* Three different methods developed and compared.
* Comparisons of a partially parallel and fully parallel PSO-GPU implementation by speed-up ratio to a serial PSO-CPU.
* Computational time ratio of PSO-GPUs to PSO-CPUs.
* Constant set of parameters used across all comparisons. 5000 optimization steps.
* 4 different algorithms for the fitness function tested. Sphere, Rosenbrock, Griewank, Ackley.

## Analysis of a single factor

* Control factor is: Number of Particles (n)
* As number of agents (n) increases, so does the speed-up ratio for PSO-GPU to PSO-CPU. This makes sense as serial vs parallel. This speedup would decrease slightly as n increases past thread count.
* Different numbers of particles tested, to show how implmentations scale.
* Fixed dimensions and thread-block size.
* The objective function values, and computing time recorded then speedup on partial and full GPU implementations calcualted.
* Computational Environment defined.

* The full GPU implementation speedup always greater than partial implementation. This shows parallelisng all steps possible provides greater performance than just the fitness calculation.
* Apparently the optimal values for the GPU implementations is same/similar(f=0) to CPU and therefore doesn't achieve performance at expense of accuracy(?)
* It can be seen speedup of Full GPU implementation increases as n increases. Showing it handles larger particles than CPU.
* Sometimes partial GPU is worse than CPU. This could be because of all the other parts calcualted on CPU have a transfer cost between the CPU and GPU which exceeds gains through fitness calculation on the GPU.
* at somepoint increasing n will see speedup increase being reduced. As the number of cores/threads is exceeded. This was hypothesied and expected.

## Analysis of Multiple control factors

* Control factors are: Number of particles (n), dimension (d), thread block-size (s)
* Can go into detail how each factor effects speedup. And which has greatest impact.

## Trajectory Optimization



## Personal Reflection

* A big criticism for this paper is that it really is a comparison of a nieve serial method against parallelization. A really valuable study would have compared a parallel simd process on the CPU against a the cuda GPU implementation.
* Possible alterantives to CUDA for general purpose comptuing on the GPU would be OpenCL or even mapping algorithm into graphical concepts and processing through graphics pipeline. Like in OpenGL or vulkan or Direct3D.
* Could model agents within a 1D graphics texture, the RGBA values could be the RGB as veloctiy and alpha channel as position. Each fragment can read neighbouring agents values.
* Programming the fitness function into GPU shader/kernel is direct when you programming the whole framework. But a accessible way needs to be provided if this was to become available to developers. Need considering if fitness function depends on external values/parameters.
* Optimal if number of threads(q) is equal to or exceeds number of agents(n) (p >= n). Otherwise if number of agents begins to surpass thread count, performance is expected to drop.
* Highlight important aspects to consider when programming for SIMD. Cache alingment. Using the right memory (local, private, global, buffer)
* A really good thing they did is by using four different PSO algorithms in each method. Time analysis of each and comparing.
* Not only do they parallelize the fitness function evaluation. But the initalization and 