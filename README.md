# IntelEdgeAINenodegreeNotes

Low Latency: Faster Response
Latency = delay

# Hardwares:

## FPGA Specifications:

High performance, low latency. Once programmed with a suitable bitstream, FPGAs can execute neural networks with high performance and very little latency. The high performance comes from the ability to run many sections of the FPGA in parallel. FPGAs also flow the data from one layer to the next, while keeping the data from one output to the next input layer on the same FPGA device. When running a neural network, we run the whole thing on the FPGA so the FPGAs don't go off-chip for the memory. This is faster than sending the output back to the CPU over the PCIe bus.

### Advantages: 
* Flexibility. FPGAs are flexible in a few different ways:
* They are field-programmable; they can be reprogrammed to adapt to new, evolving, and custom networks
* Various precision options (FP16, 11 and 9 bit ) are supported—allowing developers a balance between speed and accuracy.
* The bitstreams being used can be updated without changing the hardware. This allows you to improve the performance of your system without replacing the FPGA.
* Large Networks. One feature of FPGAs that makes them especially useful in deep learning is that they can support large networks, with a capacity to handle networks that have more than 2 million parameters.
* Robust. FPGAs are designed to have 100% on-time performance, meaning they can be continuously running 24 hours a day, 7 days a week, 365 days a year. They are also able to function over a wide range of temperatures, from 0° C to 60° C. This means that FPGAs can be deployed in harsh environments like factory floors and still perform optimally.
* Long Lifespan. FPGAs have a long lifespan. For example, FPGAs that use devices from Intel’s Internet of Things Group have a guaranteed availability of 10 years, from start of production.
