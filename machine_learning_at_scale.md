---
layout: post
title: Machine Learning At Scale : 9 Performance Tips
description:   
nav-menu: yes
tags:
 - Data Science
 - Machine learning
 - Statistics
---

# Machine Learning At Scale : 9 Performance Tips

### TL;DR
Data scientists should think about the time complexity of running programs along with how to distribute workloads. Most naive methods won’t work or require extremely expensive equipment to run properly. Instead consider these 9 Tips to improve Machine Learning Algorithms at Scale:

Trade-off accuracy for simplicity.
Think parallel and distributed implementations.
Forget simulation unless you can prove bounded convergence.
Build models step wise, i.e. with implementation in mind.
Know thy time/storage complexities.
Leverage your math skills heavily to reduce time/space complexity.
If you have to invert a matrix check if there are shortcuts.
Use in memory methods and increase your RAM.
Thread or enable multi-processing.

## Why you should care?
Coming from a statistics background, I’ve always been curious about the practicality of our methods in the world of big data.
In particular scalability is something that, we as statisticians, generally take for granted – arguing in favor of horizontal and vertical scalability. This is an argument I have never found to be satisfactory.

For example suppose you are an Uberesque company that provides geo-location services to drivers and customers in a large city. Ideally you would like to calculate the distances between your drivers, passengers and other chauffeurs. At peak hours, in a place like New York, there may be $10^5$ combined taxis and passengers on the road. Calculating the distance between one passenger and other pickups/taxis results in one row of $10^5$ distances. This is repeated for each passenger and chauffeur. The resulting “distance matrix” would therefore be a square $10^5 x 10^5$ matrix, that’s over $10^{10}$ data points or ~10Gb assuming each entry is a double precision number(64bits). Which may be trivially small, however if it needs to be updated at a regular cadence and stored for posterity the storage and compute requirements quickly add up.

While advanced methods can be used to reduce the storage burden, a secondary consideration is the computational limit. For example we may want to model pick-up/drop-off locations of passengers, hot spots or optimize distance to reduce wait-time. All of these need to be considered in the context of the size of the generated distance matrix and the processing power of our underlying computer. For example a applying a naive matrix inverse transformation requires $O(n^3)$ calculations. So if we wanted to invert our distance matrix it would require $O(n^{15})$ calculations. Put into number this means it would take $10^{15}/(3500000000 * 60 * 60* 24)\approx 3.3 days$ to complete assuming a 3.5Ghz core (or 3.5 billion calculations a second).

## 9 Tips for Scaling Machine Learning Algorithms
So, thinking forward how can we better train future statisticians and data scientists to think about big data problems and their computational limits? Here are suggestions:

### (1) Trade-off accuracy for simplicity.
One of the biggest arguments for making better models is an increase in accuracy. The truth however is that if it cannot be implemented at scale than a smaller model is always better. Period. What’s the point in having a model that doesn’t run on a large data set? Wasn’t the goal of statistics originally to compress data to make it more manageable?

### (2) Think parallel and distributed implementations.
Methods that can be parallelized are a major boon to the big data industry because they distribute run-time. This means horizontal scaling (provisioning more machines) results in linear decrease in run-time. The most interesting case is with GPUs and stochastic gradient descent which distributes inference and allows massive increases in speed compared with CPU bound processing.

### (3) Forget simulation unless you can prove bounded convergence.
I love simulating – it is the foundation of much of Bayesian Inference however if you’re planning on simulating a model with millions or even billions of data points you need to assure that convergence will occur within a bounded range (which as we know is really difficult). That is you have to be happy with a certain $\epsilon$ error. If you need to simulate perhaps try something like variational inference which is harder to implement and not as accurate but much quicker.

### (4) Build models step wise, i.e. with implementation in mind.
It’s fine to develop a theoretical model all at once but I think a better approach is to iteratively build a model and ask where and what can be parallelized or approximated to reduce time/storage complexity. Your model may not reach the global maxima but a good local maximum is fine too. I’m thinking about this particularly in cases like boosted models where you can layer methods to achieve better results.

### (5) Know thy time/storage complexities.
Statisticians frequently deal with asymptotic approximations. Port this skill set to stochastic algorithms which use asymptotic arguments for storage and run-time rather than bounding error. I’d argue that learning how different sort and search methods work along with their storage and run-time complexity are key to building scalable methods.

### (6) Leverage your math skills heavily to reduce time/space complexity.
One of the most interesting experiences I had during my dissertation was developing methods to dramatically reduce storage and run-time complexity for huge multivariate random fields.
For practitioners who are familiar with this class of models simulating any Gaussian field over 10000 points becomes computationally intractable (think $\Omega(n^{2.76})$ complexity. Ultimately leveraging functional analysis my method scaled in a $O(n\log(n))$ time, that is I could simulate a 250000 dimensional Gaussian Random Field in a matter of seconds rather than a couple of hours or days. It’s the difference between building a house in a day vs a year.
The lesson is using math intelligently can dramatically simplify your approach.

### (7) If you have to invert a matrix check if there are shortcuts.
Inverting and storing a large matrix is the worst, however if you have to do it here are some shortcuts you can take to make your experience less painful.
– Get the right MKL, BLAS, OpenBLAS or CUDA libraries and integrate them with your statistical programming language. Not all systems have optimized backends (OSx comes pre-installed with MKL – thank you Mac!).
– If your matrix is sparse don’t store the entire matrix, rather store only those elements you need. Most programming languages have some sort of sparse matrix representation. This is particularly important in natural language programming where dictionary generation, if naively implemented, can exceed your location machines space.

### (8) Use in memory methods and increase your RAM.
RAM stands for random access memory and is sometimes called volatile memory. Computers usually process information either on disk or in RAM. RAM is temporary memory that gets reset/erased after your computer is shutdown whereas disk memory persistently stores your information. RAM is dramatically quicker at accessing information than disk memory. When using your computer programs this is where most of your information is temporarily stored when code is being executed. Certain programming languages only use in-memory by default, such as <code> R </code>, whereas others use a combination of both. When a program exclusively uses RAM it runs the risk of depleting your resources and causing a system crash. Alternatively scripting languages like <code> python </code> use a combination of both. When <code> python </code> writes to disk it doesn’t crash the program however can dramatically slow down your system as a whole since it needs to write to disk and find where the memory blocks are located in memory. New solid state drives provide an increase in memory throughput to the CPU however are still not as quick as RAM. So remember the more RAM you have to play with the quicker your programs will run.

### (9) Thread or enable multi-processing.
This is building off of point (8) once you have enough memory to store your information you will need your CPU to process the information. Traditionally more GHz implies more power. While this is true the biggest bang you will get for your buck is by threading or enabling multi-processing in your code. Most modern computer languages allow you to enable this however you have to usually tell your code when to use parallel processing (it’s not a panacea – not all code can be parallelized).


