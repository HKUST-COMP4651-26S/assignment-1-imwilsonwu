[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/aQEb8Lmc)
# COMP4651 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Saturday

---

### Name: WU, Minghua
### Student Id:  20897954
### Email: mwuba@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > sysbench is used to measure the performance. 

    > In terms of CPU, I set the parameter to be `sysbench cpu --threads=4 --time=30 run`. In specific, the number of thread is set to 4 (instead of the default value 1) to make sure multiple CPU cores are fully utilized (especially for t2.medium and c5d.large which contain multiple cpu cores). The total testing time is set to 30s for a more stable result. Other parameter (e.g. max prime) is kept as default (10000). The result is the number of events per second, which represent the number of events ( complex prime computation tasks) that can be completed in one seconds. Higher value means better performance (i.e. can finish more computation in one unit of time). 

    > In terms of Memory, I set the paramter to be `sysbench memory --threads=4 run`. In specific, the number of thread is, again, set to 4 to make sure the parallel performance is measured correctly. Other parameters are set to the default value. The total size of memory transfer during the process is 100G and the memory block size is set to 1K, which is similar to the memory size of real-life application. The result is the size of memory (in MiB) that is transfered in one second. Higher value means faster memory and better performance. 

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    > The performance of EC2 does NOT increase commensurate with the number of vCPUs and memory resource. The architecture of the instance may be another important factor that need to be considered. For instances with similar architecture and type (e.g. t2.micro and t2.medium), the increase of the number of vCPUs and memory can indeed increase overall performance, by parallelism (multi threads). However, for instances with different architectures, we cannot directly compare vCPU and memory resource. For example, the c5d.large instance, which has the same number of vCPU and memory size as the t2.medium instance, has worse CPU performance but much better memory performance than the t2.medium instance. 

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance (events/sec) | Memory performance (MiB/sec) |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` |     2308.66            |     528.55               |
    | `t2.medium`  |   3917.10        |      838.42      |
    | `c5d.large` |    1806.38       |     7655.28      |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |     4890       |     0.313     |
    | `m5.large` - `m5.large`   |     4970       |     0.152    |
    | `c5n.large` - `c5n.large` |     4940       |     0.141   |
    | `t3.medium` - `c5n.large` |     4890       |     0.558  |
    | `m5.large` - `c5n.large`  |     4940       |     0.657    |
    | `m5.large` - `t3.medium`  |     4920       |     0.933   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |        522     |   54.8     |
    | N. Virginia - N. Virginia |       4760    |   0.244  |
    | Oregon - Oregon           |       4770       |   0.131    |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
