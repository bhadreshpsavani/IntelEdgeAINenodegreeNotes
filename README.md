# IntelEdgeAINenodegreeNotes

Low Latency: Faster Response
Latency = delay

# Hardwares:

## Integrated GPUs:
An integrated GPU (IGPU) is a GPU that is located on a processor alongside the CPU cores and shares memory with them.

### Key Characteristics of Integrated GPUs:
* Configurable Power Consumption: The clock rate for the slice and unslice can be controlled separately. This means that unused sections in a GPU can be powered down to reduce power consumption.
* OpenCL Startup Time: When loading a model, OpenCL uses a **just-in-time compiler** to compile the code for whatever hardware is being used. With an IGPU, this can lead to significantly **longer model load times** when compared to the same OpenVINO application running on just a CPU.
* Model Precision and Speed: On IGPUs, the Execution Unit instruction set and hardware are optimized for 16bit floating point data types. This improves inference speed, as we can process twice as many **16bit** operands per clock cycle as we can when using 32 bit operands.
* Shared Components: The CPU and IGPU are present on the same die and they share the same system memory, higher-level caches, and memory controller. This reduces memory latency by speeding up data transfer between the two devices.

## Intel Neural Compute Stick 2:
The Neural Compute Stick 2 (NCS2) is a USB3.1 plug and play removable VPU for AI inferencing.

### The key features of the Intel NCS2:

* VPU: The processor in the NCS2 is the Myriad X VPU.
* Software development kit: With the integration of OpenVino Toolkit the Intel NCS2 offers pre-trained models to be run on the stick. This allows ease in the use of the hardware.
* Operating System: The NCS2 supports all of the same operating systems as OpenVINO, including Ubuntu, Windows 10, and MacOS.
* Precision: The NCS2 only supports **FP16** model precision.
* Interface: The NCS2 has a convenient USB3.1 plug and play interface. Note that the NCS2 can be used on systems with only a USB2 port, but the inference will run slower due to I/O throttling.
* Cost: Compared to other AI accelerators, the NCS2 is an inexpensive option, typically costing around **$70 to $100**.
* Scalability: Adding multiple NCS2s (or other Myriad X devices) will allow multiple inferences to run in parallel.
* Size: All of these features come in a **small size* of 72.5mm X 27mm X 14mm, with the looks of a standard thumb drive.

### FPS vs. Power Tradeoff
One other characteristic that is important to note about the NCS2 is that it is meant to be a low-power device so that it can be easily deployed at the edge; however, one drawback of this is that it cannot process as many frames per second (FPS) as some other devices and thus it has a higher inference time.

For example, if we compare the NCS2 with an Atom E3950 processor, we can see that the Atom will have better FPS, but higher power requirements. The Atom processor (even though it is a relatively low-power processor) has about 12 times the power requirements of the NCS2. Thus, there is a tradeoff between power requirements and performance; the NCS2 is extremely low power, but this can come at some cost to performance as compared to the Atom

## FPGA Specifications:

High performance, low latency. Once programmed with a suitable bitstream, FPGAs can execute neural networks with high performance and very little latency. The high performance comes from the ability to run many sections of the FPGA in parallel. FPGAs also flow the data from one layer to the next, while keeping the data from one output to the next input layer on the same FPGA device. When running a neural network, we run the whole thing on the FPGA so the FPGAs don't go off-chip for the memory. This is faster than sending the output back to the CPU over the PCIe bus.

### Advantages: 
* Flexibility: FPGAs are flexible in a few different ways, They are field-programmable; they can be *reprogrammed* to adapt to new, evolving, and custom networks. Various precision options **(FP16, 11 and 9 bit )** are supported—allowing developers a balance between speed and accuracy.
* The bitstreams being used can be updated without changing the hardware. This allows you to improve the performance of your system without replacing the FPGA.
* Large Networks: One feature of FPGAs that makes them especially useful in deep learning is that they can support large networks, with a capacity to handle networks that have more than 2 million parameters.
* Robust: FPGAs are designed to have 100% on-time performance, meaning they can be continuously running 24 hours a day, 7 days a week, 365 days a year. They are also able to function over a wide range of temperatures, from **0° C to 60° C**. This means that FPGAs can be deployed in harsh environments like factory floors and still perform optimally.
* Long Lifespan: FPGAs have a **long lifespan**. For example, FPGAs that use devices from Intel’s Internet of Things Group have a guaranteed availability of 10 years, from start of production.


## Reducing Model Size:

we'll be covering different techniques for benchmarking model size and reducing model size. Here are the main topics we'll discuss:

* Quantization
* DL Workbench
    - Benchmark Models
    - Quantize Models
* Weight Sharing
    - Zip a model
* Knowledge Distillation

### Quantization: 

In this lesson, we will be learning about one of the most widely used and researched techniques for reducing model size: quantization.

"
Broadly speaking, quantization is the process of mapping values from a larger set to a smaller one. In quantization, we might start off with a continuous (and perhaps infinite) number of possible values, and map these to a smaller (finite) set of values. In other words, quantization is the process of reducing large numbers of continuous values down into a limited number of discrete quantities.
"

In the present context, we can use quantization to map very large numbers of high-precision weight values to a lower number of lower-precision weight values—thus reducing the size of the model by reducing the number of bits we use to store each weight.

TYPE OF NEURAL NETWORK:
1. INT8 Quantized Nerual network
2. Binary Neural Network
3. Ternary Neural Network

[XNOR-Net: ImageNet Classification Using Binary Convolutional Neural Networks (Rastegari et al., 2016)](https://video.udacity-data.com/topher/2020/March/5e6e8859_xnor-net-imagenet-classification-using-binary-convolutional-neural-networks/xnor-net-imagenet-classification-using-binary-convolutional-neural-networks.pdf)

DL Workbench can help us get accurate performance metrics about our model. But there are a few points that you should keep in mind when interpreting the results you get:

DL Workbench: 
DL Workbench can help us get accurate performance metrics about our model. But there are a few points that you should keep in mind when interpreting the results you get:

1. **DL Workbench only gives performance results of your model and not your application code.** There might be bottlenecks in your application code that could cause your system to perform poorly. 
2. **DL Workbench gives you performance results only for the hardware you select.** You might get different results for different hardware.
3. **Even though the model might give you the best performance at a certain value of batches and streams for a given hardware, your scenario may not generate data fast enough to fulfill those batch and stream requirements.** In a situation like this, your model will have to wait for data, which might offset the performance gains of batching and streaming. You should therefore select the batch and stream sizes that takes into account not only the throughput and latency, but also how data is generated in your application.
