#PolarDB with computational storage
##Background
1. Cloud vendors provide cloud native relational database service (RDS) business, like Alibaba PolarDB and Amazon Aurora.
3. In order to achieve scalability and fault resilience, cloud-native relational databases naturally follow the design principle of **decoupling compute from data storage**.
4. It is highly desirable for cloud-native relational databases to adequately support analytical workloads.
##Issues
1. Decoupling compute from data storage, the network bandwidth between database nodes (**computing**) and storage nodes (**storage**) becomes a scarce resource.
2. PolarDB architecture:
![image](https://github.com/humasama/technologies/blob/main/PolarDB-Arch.png)

