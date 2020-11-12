+++
author = "Neha Joshi"
title = "NVIDIA Multi-Instance GPU"
date = 2020-09-05T18:05:08+05:30
draft = false
description = "NVIDIA Multi-Instance GPU – Accelerating the virtualized cloud computing datacenter"
tags = [
    "Cloud computing",
    "Data Center",
    "NVIDIA Ampere",
    "GPU",
    "Virtualization"
]
+++

I worked on early development and testing of MIG support on vGPU, and was involved in the emulation and bringup of the Ampere GPU.

## Background

Modern cloud computing datacenters rely on GPUs to deliver unparalleled performance for accelerated computing workloads such as AI, ML, etc. Workloads run in Virtual Machines (VMs) for security and management efficiency. So efficiently running VM with GPU is essential. As GPUs have increased in performance, many workloads no longer require a full GPU, so must share a GPU to maintain efficiency. Pre-Ampere architectures sequentially timeshare the GPU between the VMs, with each workload running for a few milliseconds. The GPU remains underutilized since none of these workloads fill it, and the workloads take longer to run as they wait for their timeshare. With Ampere, MIG makes it possible for the workloads to run in parallel on smaller fractions of the GPU, rather than timeshared on the full GPU.
MIG allows GPUs to more efficiently address wider range of datacenter workloads by optimizing the GPU utilization.

## Introduction to GPU virtualization

The NVIDIA vGPU is NVIDIA’s architectural solution for GPU virtualization. Virtualization just creates virtual barriers between multiple virtual environments running on the same physical environment. So, the actual physical devices may manifest themselves as virtual devices, which may actually be a portion of the physical device.
The primary benefit of the MIG feature is increasing GPU utilization by enabling the GPU to be shared effectively by parallel compute workloads on bare metal or on multiple vGPUs.

The MIG feature centers on the following primary concepts:

- [1] **GPU instance**

- Creating GPU instances can be thought of as splitting one big GPU into multiple smaller GPUs (GPU instances) as each GPU instance has dedicated caches and memory bandwidth and dedicated compute resources

- [2] **GPU Compute instance**

- A GPU Compute instance is created under a GPU instance and encapsulates all the Compute resources that can execute work on the GPU.

- [3] **GPU slice**

- A GPU slice is the smallest fraction of the Ampere GPU. Each “GPU Slice” includes a “Sys Pipe”, one GPC, one L2 slice group, and access to a portion of frame buffer memory. The Ampere GPU supports a total
of 7 GPU slices.


## {{< figure src="/images/GPU_slice1.png" title="GPU virtualization" width=500 height=500 >}}

# Benefits
- ✓ Fault isolation
- ✓ Performance isolation
- ✓ Memory bandwidth isolation

## GPU INSTANCE:
## {{< figure src="/images/GPU_instance.png" height=400 width=400 >}}

The above diagram shows the possible ways in which the GPU can be partitioned. At any given time, the GPU can be partitioned into any number of ways, as long as the blocks don’t overlap.

## GPU COMPUTE INSTANCE:
Once the GPU instance is assigned to a VM, upon VM bootup, the user can:
- ✓ Query the available compute instances
- ✓ Set the required compute instances

Setting the required compute instances depends on whether the user wishes to execute a single large workload, or multiple smaller workloads which are independent.
- Example: In case of container environment, we have multiple independent applications which can run in parallel. In that case, choosing the right number of compute instances to get these truly independent applications execute in parallel is a key to maximum utilization and performance.

## ASSIGNING GPU PARTITIONS TO A VM:
## {{< figure src="/images/MIG.png" height=650 width=650 >}}

## CONCLUSION:
The beauty of MIG is that, all the VMs will now run in parallel on the Ampere. Any issues in VM-1 will not affect the other two VMs. Also, if VM-2 is executing any memory-intensive workload, then that will not affect the VM-1 and VM-3 in any way.
Compute applications treat a “Compute instance” and its parent “GPU instance” as a single (smaller) GPU. The user can choose to execute the compute workload on any of the compute instances which are created in the GPU instance.
Thus, **MIG enables improved GPU utilization by enabling the GPU to be shared effectively by truly parallel compute workloads**.

# REFERENCES:
- ✓ Inside NVIDIA's Multi-Instance GPU Feature - Jay Duluk, NVIDIA | Piotr Jaroszynski, NVIDIA
GTC 2020 (https://developer.nvidia.com/gtc/2020/video/s21975-vid)

- ✓ Delivering High-Performance Remote Graphics with NVIDIA GRID Virtual GPU – Andy Currid, NVIDIA, GTC 2014 (http://on-demand.gputechconf.com/gtc/2014/presentations/S4725-hi-perf-graphics-nvidia-grid-virtual-gpus.pdf)

### Thank you [Andy Currid](https://www.linkedin.com/in/andycurrid/), [Neo Jia](https://www.linkedin.com/in/neojia/) and [Kirti Wankhede](https://www.linkedin.com/in/kirti-wankhede-87249520/) for reviewing the early drafts of this blogpost!

Happy Coding!