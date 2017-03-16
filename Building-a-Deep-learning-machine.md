---
layout: post
title: Building a deep learning machine 
description:   
image: assets/images/pic11.jpg
nav-menu: yes
tags:
 - Statistics
 - Machine Learning
 - Algorithms
 - Big Data
---

## Background
Building a deep learning machine is costly. There is no way around (well at least at the time of this writing) but with some thought you can minimize cost and maximize your power. You can achieve speeds of roughly 36Teraflop per second (TFLOPS) or 1/5 the speed of $150K NVidia's custom DGX-1 at about 1/50th~2\% the cost (~$3000/$150000 = 0.02) by using the appropriate components.

## Word of warning
If you have never built a computer before this isn't meant to be a primer. A great resource for getting started is at asdasd youtube channel. They explain the different components and how they come together. While the channel emphasizes development for a gaming rig, building a deep learning machine is different given the parameters we're optimizing.

## Thoroughness
Tim Dettmers wrote a fantastic article on [hardware for deep learning](http://timdettmers.com/2015/03/09/deep-learning-hardware-guide/) it is more technical.
I have done my best to include elements that are relevant to beginners, this is aliving document so will update it from time to time as new technology comes out or major revisions are needed.

## 0. The right parts
When building a deep learning rig you will need the following components:
 1. Motherboard
 2. CPU
 3. RAM
 4. GPU
 5. Power supply unit
 6. Cooler

## 1. Power Supply Unit (PSU)
Historically you would have needed to buy a power supply unit that could handle loads of >1200 Watts to run your deep learning machine, most of which would have been used by your GPUs. For example old GPU archicectures, such as Titan X's Maxwell architecture, required ~400W to run. After the release of the 10X0 series the gb/wattage requirement has been more than halved. At peak loads most 10X0 series GPUs will run at 200W see [here](http://www.tomshardware.com/reviews/nvidia-geforce-gtx-1080-pascal,4572-10.html). The remainder of the Wattage is distributed across your CPU, storage, memory and cooling system. Of the four the CPU consumes the most power. Most modern processors will consumer between 75W-120W. For example the Intel I5 6600K chip comes out at 91W. In some cases the chipset may have the option to consume more - this is traditionally used by over-clockers, so not relevant here.

In either case, recent advances in GPUs have dramatiaclly decreased the total power consumption requirement which lowers the power burden. A good rule of thumb is that you'll need a power supply with 200W + 200W/GPU. So for a 4 GPU build you'll minimally require a 1050W PSU.

## 2. Motherboards
If the CPU is the brain of the computer then the motherboard is nervous system. It is the piece of equipement that holds your components together and coordinates processing between RAM, GPU, CPU and storage. There are a couple of things you need to watch out for with a motherboard, they are:
 1. The number of PCIe lanes
 2. CPU support
 3. Support for SSD M.2 NVME
 4. Design/layout of components

## 2.1 PCIe lanes
PCIe stands for __Peripheral Component Interconnect Express__. It is a serial expansion bus standard for connecting a computer to one or more peripheral. In short this is where your graphics cards plugs into the motherboard.  There are different  varieties of PCIe lanes and consequently different speeds. As of 2016 PCIe 3.1 16x are the fastest. The 16x is the number of lanes that are accessable for data transfer. These types of PCIe's have transfer speeds of up to 32Gb/second. Other versions are slower.
 PCIe lanes are updated every couple of years to reflect new technological improvements. The most recent update occured in November 2014 from the 3.0 to 3.1 to reflect power management, performance and functionality improvements. The implication is that if you have an end of year 2014 or early 2015 motherboard you may be working with older technology. A PCIe update (v4.0) is slated for 2017 and looks to dramatically increase the transfer rate. 

Motherboards do not come with a standard number of PCIe slots. The more that are supported the more GPUs you can plug into your motherboard and accordingly get better performance. It's possible to get upto 6 3.0 16x PCIe lanes implying that you should be able to have 6 supported GPUs. That being said the cost differential between 3 and 4 PCIe slots tends to be minor whereas cost differences are significant once you reach 5. Therefore you should aim to have at least 4. If you can buy a motherbaord with 5, just make sure that there is sufficient space between slots so that you can stack your GPUs (sometimes motherboards will place them closer together for other peripheral devices that don't occupy as much room). In addition you may find that once your reach 5 PCIe slots that you need a more computing power (hence more expensive) CPU to handle the load. As of writing the main motherboards that I'm aware of that have 5PCIe lanes require Intel X99 chips.

## 2.2 CPU support
One thing most novice builders fail to account for is the supported CPU architecture. I have built custom desktops twice before and at each iteration I check to make sure my motherboard supports my CPU type. For example certain motherboards only support particular intel architectures i.e. Z170 and X99 
 You will also want to make sure that your CPU supports PCIe 3.1 lanes, some don't (although uncommon with newer builds). You'll also want to make sure that you have the right CPU with the right motherboard. 

I've traditionally supported Intel CPUs rather than AMDs. AMD processors tend to be cheaper than similarly clocked Intels however get dramatically hotter, impacting performance. In addition AMD bought ATI, NVidia's competitor in 2006, so has focused on graphics support for ATI rather NVidia cards. 

## 2.3 NVME M.2 SSD
The newest type of storage is called solid state storage (SSD). It has no moving parts (so won't break if dropped) and is much quicker than old disk spinning hard drives. Traditional HDDs (hard disk drive) have a read/write speed of about 100mb/s which erodes as the space on your hard drive is filled. In contrast Serial AT Attachement (SATA) express SSD's have a read/write of 500mb/s, x5 times faster than traditional HDDs. 
The newest storage unit is the __next generation form factor non-volatile memory express__ or M.2 NVME. These types of storage devices have all the same advantages as SATAe ssd's however have read/write speeds >1600mb/s. While M.2 NVME ssds arguably provide the best performance they are significantly more expensive than other storage formats. 
One of the main drawbacks to these storage devices is that most motherboards have limited nuber of support slots (usually 4). Accordingly if you wanted 4 Terabytes worth of space you would need to buy 4x1TB drives instead of ammortizing over smaller and less expensive drives (i.e. 8 SATA ssds). At the moment Samsung is offering some good deals on these devices which can be found [here](https://www.newegg.com/Product/Product.aspx?Item=N82E16820147595&cm_re=nvme-_-20-147-595-_-Product). They are priced to be on par with their slower and older generation SSDs. They however tend to be back-ordered.

## 2.4 Design/layout of components
Graphics cards are big so when you purchase a motherboard make sure it's layout allows sufficient space to install and ventilate your graphics cards. This may seem trivial however overheating is a serious problem. Grahics cards shutdown around 105C/221F and most computer manufacturers rarely assume you'll be using all PCIe lanes. The more space you have between PCIe slots the better ventilated your GPUs will be, leading to lower tempratures thus preventing overheating.

## 3. Graphics Cards
### 3.1 Why do we need a GPU?
Among the reasons why Deep Learning has been able to move so quickly in the last couple of years is because of the use of consumer graphics processing unit (GPU or simpy graphics card) to massively parallelize calculations. In contrast to CPU's which run commands sequentially at high speeds, GPU perform computations over more cores that are slower. Since most deep learning frameworks are parallelizable GPU accelerated learning will run significantly quicker than on CPUs. A recent paper by Shi, Wanhg, Shu and Chu compares different DL frameworks on a single desktop CPU, single server CPU and single GPU. The paper shows that a single GPU can achieve almost 50x increase in speed compared to desktop CPUs.

### 3.2 What GPU brand should I buy?
The GPUs are the most important part of your Deep Learning build since it is here that computation occurs. There is a tradeoff between speed, memory and cost. There is currently only one brand-supported GPU (NVidia) that has heavily invested in deep-learning architecture. They have released several proprietary libraries (cuDNN, CUDA) to optimize consumer grade GPUs for deep learning  ATI is working on their Vega architeture/chip which will enable similar features however their technology trails behind NVidia; so sticking with NVidia GPUs is the safest bet for the next couple of years.

### 3.3 What GPU Cards should I buy?
If you wanted to max out your local rig you could opt to buy 4 Titan X (Pascal architectures) however the increase in speed would not be commiserate with the increase in price versus other options. Below are the costs for prototypical cards are included, along with their wattage, CUDA cores, RAM and Teraflops. 
- Titan X (Pascal architectures) - $1200 MSRP, 250W, 3584, 12 GBDDR5X, 11 TFLOPs
- GTX 1080 (Pascal architecture) - $599 MSRP, 180W, 2560, 8GB GDDR5X, 8.9 TFLOPs
- GTX 1070 (Pascal architecture) - $379 MSRP, 150W, 1920, 8GB GDDR5, 6.5 TFLOPs
- GTX 1060 (Pascal architecture) - $300 MSRP, 120W, 1280, 6GB GDDR5, 4.4 TFLOPs
For a full list of features check out [NVidia's site](https://www.nvidia.com/en-us/geforce/products/10series) and [WCCFTECG](http://wccftech.com/nvidia-geforce-gtx-1060-final-specifications/).

#### 3.3.1 So what do all these features mean?
Wattage is the amount of power your card consumes. CUDA cores are the locations where the calculations take place. CUDA cores determine how many parallel calculations can take place, i.e. more cores = more parallel computations. Memory is important for distriubting batches of data. The more memory you have available to your card the less data is transferred between disk, RAM and CPU which are much slower since processing is run in queues rather than in parallel.
TFLOPs are the number of calculations (in trillions) your GPU performs in a second. Generally you want to choose a card that balances all components. 

I have found for the price the GTX 1070 offers the best solution. Purchasing 4 GTXs will cost you $1500 whereas 4 GTX 1080s will be $2400. The difference in performance will be 26 TFLOPs to 35.6 TFLOPs (GTX 1080 is about 1.37x faster than GTX 1070), however the GTX 1080 is about 20% more expensive for the price.

#### 3.3.2 Benchmarks and Cost
Comparing these cards solely on specs is not entirely fair since the Titan X architecture allows for larger batch sizes. A speed comparision using [DeepMark](https://github.com/DeepMark/deepmark) for the different cards on MxNet (open source Deep Learning framework support by Amazon) can be found at the [DMLC website](http://dmlc.ml/mxnet/2016/08/03/mxnet-titanx-benchmark.html). If you are wondering about some of the older cards a comparison for image classification can be found [here](http://mxnet.io/tutorials/computer_vision/image_classification.html)

So how does this compare to the DGX-1 (170TFLOPs at $150K). Here's the breakdown of speeds for a 4-way multigpu GTX 10X0 card setup looking only at GPU costs not including MB, RAM, PSU,... costs:
- GTX 1080 15% speed (26TFLOPs/170TFLOPs), 1% the cost DGX-1 
- GTX 1080 21% speed (35.6TFLOPs/170TFLOPs), 1.6% the cost DGX-1 
- Titan X 26% speed (44TFLOPs/170TFLOPs), 3.2% the cost DGX-1 


#### 3.4 Founder's Edition vs Generic Edition
Founder's edition GPUs (also known as "Reference cards") are simply cards that adhere to NVidia's design specs. They are usually released earlier than generic edition and consequently cost more. The online digital magazine [The Verge](http://www.theverge.com/) has indicated that FE cards are base confiugrations that are not optimized for speed or functionliaty so may get hotter and consequently perform worse than competing generic editions which modify design (more fans, bigger heatsync) and functionality (allow overclocking). For full details see [here](http://www.theverge.com/circuitbreaker/2016/5/12/11662346/nvidia-founders-edition-geforce-gtx-explained). 
That is to say buying founder's edition cards are probably not worth the investment unless you are impatient or have the cash. 

## 4 What components matter for deep learning?
Since most of the compute occurs on the GPUs having a powerful CPU will not add (much) to speed, nor will necessarily buying the most expensive RAM. Consider the different transfer speeds for RAM:
DDR4 data transfer rate:
- DDR4 2133：17 GB/s
- DDR4 2400：19.2 GB/s
- DDR4 2666：21.3 GB/s
- DDR4 3200：25.6 GB/s

We can calculate the total RAM bandwidth by multiplying the numbers above by the number of channels your motherboard supports, we use the formula below for a 64-bit architecture:
'''
bandwidth GB/S = RAM clock in GHz x Memory Channels of CPU x 64 x 8^{-1}
'''
So for example suppose you had a DDR4 2400 RAM then your bandwidth in GB/S would be 2.4 x 4 x 64 / 8 = 76.8GB/s or 19.2GB/s x 4 = 76.8GB/s.

Considering that you need to load datasets into memory and transfer them between the RAM and GPU for updates it is unlikely you will ever fully engage your RAM. Remember, however that you'll be running other programs in the background along with your operating system so is always better to have more than less RAM. A good rull of thumb is to have twice as much RAM as GPU memory.

### Bottlenecks
The main areas where bottlenecks occur are along the CPU <-> RAM <-> GPU pipeline. The CPU can only communicate with a GPU through direct memory access (DMA) where RAM is the intermediate step. DMA can be thought of the mechanism that allows "memory to memory" transfer when the CPU is fully engaged with other processes. The mechanism underlying DMA is involved and can be learnt about more [here](https://en.wikipedia.org/wiki/Direct_memory_access). At a high-level it is the mechanism through which updates occur across your GPUs and are synchronized through the CPU.

Since computation occurs on the GPU the main time when the CPU is engaged is in synchronization of values across GPUs (i.e. when GPUs need to talk to one another). This message passing is usually sufficiently light-weight that buying high-end CPUs won't dramatically decrease run-time (as compared with buying a higher-end GPU). Given a sufficient amount of RAM, you'll usually however find that your CPU is fully engaged which is due to the 

### Notes about GPUs
Some deep learning frameworks do not support different GPUs, as they may have different hardware components or internal configurations that are not compatable with one another. For example  
Accordingly, if you are just beginning do not mix graphics card.

## What should I purchase?
We've now gone over the basic components the question however arises what should I purchase? Below is a breakdown with associated costs (at the time of this writing) of how I've put together my deep learning machine:

__Case:__ [DEEPCOOL GENOME II  Liquid Cooling System](https://www.newegg.com/Product/ProductList.aspx?Submit=ENE&DEPA=0&Order=BESTMATCH&Description=liquid+cooling&ignorear=0&N=-1&isNodeId=1)$200-$250
This case includes a liquid cooling system along with 2 sets of fans. It is more expensive that traditional fans but takes the burden out of having to setup a liquid cooled system.

__RAM:__ [G.SKILL Aegis 64GB (4 x 16GB) DDR4 2400](https://www.newegg.com/Product/Product.aspx?item=N82E16820232255)$400-$450

__CPU:__[Intel Core i7-7700K Kaby Lake Quad-Core 4.2 GHz LGA 115](https://www.newegg.com/Product/Product.aspx?Item=N82E16819117726&cm_re=intel_i7-_-19-117-726-_-Product)$350

__Motherboard__:


