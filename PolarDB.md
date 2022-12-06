#PolarDB with computational storage
##Background
1. Cloud vendors provide cloud native relational database service (RDS) business, like Alibaba PolarDB and Amazon Aurora.
3. In order to achieve scalability and fault resilience, cloud-native relational databases naturally follow the design principle of **decoupling compute from data storage**.
4. It is highly desirable for cloud-native relational databases to adequately support analytical workloads.
##Issues
1. Decoupling compute from data storage, the network bandwidth between database nodes (**computing**) and storage nodes (**storage**) becomes a scarce resource.
2. PolarDB architecture:
![image](https://user-images.githubusercontent.com/6119088/205944979-f4e13d84-7fdd-43b7-b600-c25d4b608021.png)

