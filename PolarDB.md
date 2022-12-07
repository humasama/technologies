# PolarDB with computational storage
## Background
1. Cloud vendors provide cloud native relational database service (RDS) business, like Alibaba PolarDB and Amazon Aurora.
3. In order to achieve scalability and fault resilience, cloud-native relational databases naturally follow the design principle of **decoupling compute from data storage**.
4. It is highly desirable for cloud-native relational databases to adequately support analytical workloads.
## Issue
1. Decoupling compute from data storage, the network bandwidth between database nodes (**computing**) and storage nodes (**storage**) becomes a scarce resource.
- Current PolarDB architecture

![image](https://user-images.githubusercontent.com/6119088/206064456-00e7843e-413d-48c6-b883-2dbf55ab5614.png)

DB server and data chunk server connect with RDMA

## Known Solution
In order to better serve analytical workloads, the almost only viable option
is to **off-load data-access-intensive tasks** (in particular table
scan) from database nodes **to storage nodes**. This concept is
certainly not new and has been adopted by both proprietary
database appliances (e.g., Oracle Exadata) and open-source
databases (e.g., MySQL NDB Cluster).
### Issues
- Each storage node must be equipped with sufficient data processing power (i.e., CPU) to handle table scan tasks.
- To maintain the cost effectiveness of cloud-native databases, we
cannot significantly (or even modestly) increase the cost of
storage nodes.
### Solution - heterogeneous computing architecture
Put GPU/FPGA etc. in storage nodes.

## Two computational storage architectures
- Centrialized

![image](https://user-images.githubusercontent.com/6119088/205952357-b76dd23a-4335-4ec3-9ce8-9ed45b757541.png)

(1) High data traffic: All the raw data
in their row-store format must be fetched **from the storage
devices into the FPGA/GPU-based PCIe card**. Due to the
data-intensive nature of table scan, this leads to a very heavy
data traffic over the PCIe/DRAM channels. The high data
traffic can cause significant energy consumption overhead
and inter-workload interference.

(2) Data processing hot-spot ("hard to scale"):
Each storage node contains a large number of NVMe SSDs,
each of which can achieve multi-GB/s data read throughput.
As a result, analytical processing workloads could trigger very
high aggregated raw data access throughput that is far beyond
the I/O bandwidth of one PCIe card. This could make the
**FPGA/GPU-based PCIe card become the system bottleneck**.


- Distributed

![image](https://user-images.githubusercontent.com/6119088/205952510-2442c96e-4a63-4c84-9373-d26fa74d266e.png)

## PolarDB with computational storage SW stack

![image](https://user-images.githubusercontent.com/6119088/206061276-211293d3-e546-4032-99a4-366df88182c6.png)
- In spite of the FPGA circuit-level
programmability, it is difficult for FPGA to implement comparators
that can efficiently support multiple different data
types. In this work, we modified POLARDB storage engine
so that it stores all the table data in the memory-comparable
format, i.e., data can be compared using the function memcmp().
- Each scan engine contains one memcmp module and one RE (result evaluation) module.
- Let P = ∑m i=1(∏nj=i 1 ci;j) denote the overall scan task, where each ci;j is one individual condition on one field.
- Once the value of P (i.e., either 1 or 0) can be determined, the scan engine can immediately **finish the evaluation
on current row**, and start to work on another row.

## Questions
1. In FPGA,  it only implements type-oblivious memcmp modules to evaluate each condition. How does it work?
2. what is the basic compare size?

## Link
[1] https://www.usenix.org/conference/fast20/presentation/cao-wei
