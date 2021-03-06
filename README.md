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

### DL Workbench: 
DL Workbench can help us get accurate performance metrics about our model. But there are a few points that you should keep in mind when interpreting the results you get:

1. **DL Workbench only gives performance results of your model and not your application code.** There might be bottlenecks in your application code that could cause your system to perform poorly. 
2. **DL Workbench gives you performance results only for the hardware you select.** You might get different results for different hardware.
3. **Even though the model might give you the best performance at a certain value of batches and streams for a given hardware, your scenario may not generate data fast enough to fulfill those batch and stream requirements.** In a situation like this, your model will have to wait for data, which might offset the performance gains of batching and streaming. You should therefore select the batch and stream sizes that takes into account not only the throughput and latency, but also how data is generated in your application.

### How Quantization is Done?:

Let's get into some concrete examples of quantization, so that you can get an idea of how it is done. Remember, quantization simply refers to a mapping that reduces a larger number of values to a smaller number of values—so it can potentially be done in many different ways.

That said, the simplest form of quantization is weight quantization, in which we reduce the amount of space required for the weight values—such as by reducing our weight values from 32 bits to 8 bits.

* [A Survey on Methods and Theories of Quantized Neural Networks](https://arxiv.org/pdf/1808.04752.pdf) by Yunhui Guo, in which the author goes over some of these techniques.

* OpenVINO uses MKL-DNN, a math kernel library, to quantize their neural networks. You can find information about their quantization workflow [here](https://intel.github.io/mkl-dnn/ex_int8_simplenet.html).

### Model Compression:
One important—but often forgotten—metric that we should optimize is memory. We can do this using model compression.

Model compression refers to a group of algorithms that help us reduce the amount of memory needed to store our model, and also make our models more compact (in terms of the number of parameters).

In fact, you have already learned about a model-compression algorithm, quantization, where we reduce the number of bits required to represent each weight.

### Weight Sharing:
Another way to compress our model is to reduce the number of weights that we store. This is known as weight sharing.

In weight sharing, the goal is for multiple weights to share the same value. This reduces the number of unique weights that need to be stored, thus saving memory and reducing model size.

* [Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding (Han et al., 2016)](https://video.udacity-data.com/topher/2020/March/5e6e9c50_deep-compression-compressing-deep-neural-networks-with-pruning-trained-quantization-and-huffman-coding/deep-compression-compressing-deep-neural-networks-with-pruning-trained-quantization-and-huffman-coding.pdf)

* [Compressing Neural Networks with the Hashing Trick (Chen et al, 2015)](https://video.udacity-data.com/topher/2020/March/5e6e9c9f_compressing-neural-networks-with-the-hashing-trick/compressing-neural-networks-with-the-hashing-trick.pdf)

### Knowledge Distillation:
Knowledge distillation is a relatively newer technique to perform model compression.

Knowledge distillation is a method where we try to transfer the knowledge learned by a large, accurate model (the teacher model) to a smaller and computationally less expensive model (the student model).

Through knowledge distillation, the student model can achieve nearly the same accuracies as the teacher model by mimicking the internal representation learned by the teacher.

The generalised knowledge distillation technique we learned was introduce in a paper called [Distilling the Knowledge in a Neural Network](https://arxiv.org/abs/1503.02531) and was written by Geoffrey Hinton, Oriol Vinyals and Jeff Dean.

Knowledge Distillation is simple, but recent works have shown that it can be a very powerful method of learning. In a recent paper, researchers showed that by making some small changes to the knowledge distillation algorithm, they could improve the Top-1% Accuracy on ImageNet by 2% over the state of the art while using a model with 349M fewer parameters! If you're curious, you can check out the full paper here:

* [Self-training with Noisy Student improves ImageNet classification (Xie et al., 2020)](https://video.udacity-data.com/topher/2020/March/5e6ea044_self-training-with-noisy-student-improves-imagenet-classification/self-training-with-noisy-student-improves-imagenet-classification.pdf)

Note that the aim of the authors was actually not to compress a model, but to improve accuracy. The reduction in parameters came from using the EfficientNet architecture.

Knowledge distillation can also be combined with quantization as shown in [Model compression via distillation and quantization and with pruning](https://arxiv.org/abs/1802.05668) as shown in [Faster gaze prediction with dense networks and Fisher pruning](https://arxiv.org/abs/1801.05787).
