# PolarDB with computational storage
## Background
1. Cloud vendors provide cloud native relational database service (RDS) business, like Alibaba PolarDB and Amazon Aurora.
3. In order to achieve scalability and fault resilience, cloud-native relational databases naturally follow the design principle of **decoupling compute from data storage**.
4. It is highly desirable for cloud-native relational databases to adequately support analytical workloads.
## Issue
1. Decoupling compute from data storage, the network bandwidth between database nodes (**computing**) and storage nodes (**storage**) becomes a scarce resource.
- PolarDB architecture: DB server and data chunk server connect with RDMA
![image](https://github.com/humasama/technologies/blob/main/PolarDB-Arch.png)
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
- Centrialized: ![image](https://user-images.githubusercontent.com/6119088/205951884-f3466dea-2f71-42ae-8d24-da54c435f5df.png)
- Distributed: ![image](https://user-images.githubusercontent.com/6119088/205952017-75aaf388-d375-44df-a5c7-b3790ddcb58a.png)

