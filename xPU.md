# xPU

## Introduction

A processing unit is the electronic circuitry within a computer that carries out the instructions of a computer program by performing the basic arithmetic, logical, control and input/output (I/O) operations specified by the instructions.

In the era of deep learning and extreme complex computation day by day, the designers have created multiple specialized processing unit based on specific task. They are optimized only for the particular task and not for everything we throw at him(etc: CPU - multipurpose general processing unit).

In many professional speeches, we often hear people talking about CPU/GPU/NPU, etc. Even for engineers, they may not be familiar with them. This article will give you a look at the relationship and difference between them.

## Vendors or types or technologies

There are multiple types of processing unit especially in these few years. There are also refered to as Moores's law of AI where it is said that in every 18 days there are new name for xPU until all 28 alphabets are used.

But in this article we will only focus on few types of these common technologies:

### CPU(Central Processing Unit)

This is the most common type of processing unit currently found in all types of computers. For more detail, you can refer to an article about CPU in this github page [here](https://github.com/HokhyTann/Assignment/blob/master/cpu.md)

#### Advantages and disadvantages

Extremely fast cores, lesser cores making it bad for parallel computation but a good general computational resource. Can basically handle most task thrown at him. Lesser but extremely speedy multihireachy cache.

### GPU(Graphics Processing Unit)

GPU, also known as display core, visual processor, display chip, is a special type of personal computer, workstation, game console and some mobile devices (such as tablets, smartphones) Etc.) A microprocessor that works on image operations. The purpose is to convert and display the display information required by the computer system, and provide a line scan signal to the display to control the correct display of the display. It is an important component for connecting the display and the main board of the personal computer, and is also one of the important devices for the "human-machine dialogue". . As an important part of the computer host, the graphics card undertakes the task of outputting display graphics. It is very important for people who are engaged in professional graphic design.

#### Advantages and disadvantages

Many cores, all cores are weak, good in parallel computing, bad in serial computing. Large moderately speedy cache making it good in large space computation but bad for fast serialisable computation.

### NPU (Neural Processing Unit)

The embedded neural network processor (NPU) adopts the "data-driven parallel computing" architecture, and is especially good at processing massive multimedia data of video and image. The embedded NPU's miniaturization, low power consumption and low cost advantages accelerate the application of artificial intelligence technology. For example, the drone has high requirements on the weight and power consumption of the camera, otherwise it will affect the takeoff and endurance. The "Starlight Smart One" is only the size of a regular stamp, weighing only a few tens of grams. Its birth has enabled many small cameras such as surveillance cameras to have artificial intelligence, and has taken artificial intelligence from the mysterious computer room to the life application. One step.

#### Advantages and disadvantages

Almost like GPU but with some redundant computational components removed. Can construct its circuit like FPGA based on user behavior. Only usable in very specific task. Very slow in most condition. Very fast if used in excellent condition.

### TPU(Tensor Processing Unit)

TPU, short for Tensor Processing Unit, is a processor built by Google. It is tailor-made for machine learning. It requires fewer transistors and is more natural in performing each operation. Because it accelerates the operation of its second-generation artificial intelligence system, TensorFlow, and is much more efficient than the GPU - Google's deep neural network is driven by the TensorFlow engine.

#### Advantages and disadvantages

Very fast in specific task such as matrix computation. Very large cache making it suitable for big neural network problems. equally speedy large cores(and lots of them) designed in 3D with mind of increasing tensor(matrix)operations in a most optimum way. Lots of improvement needed to conserve energy as this processor needs a lot of energy to operate and give out a lot of heat.

### Key indicators 

Due to the complex condition of xPU, there are too many different key indicators to these processors. There are no specific indicators used by all parties as diferent processors exel in differnt ways. For example, single core speed, multicore speed, parallel computation efficiency, bubbles, energy used, processor cache size, extra functionalities, etc...

However, nowadays speed in mainly the only indicator respected by most people.

### Comments

I think someday xPU will be very powerful that most of these technologies will merge. We will be able to use same chip for different type of task in future.

I have also learnt that Intel and Google are currently trying to merge CPU and FPGA into their SoC so that these different types of processing unit wil someday usable in all devices without needing to use different type of PUs.



Meng Yit Koh 517030990022