# 1. 阿里HBase到Lindorm
## 1.1 HBase应用场景

- 高效支撑数据驱动
- 实时决策型任务
- 基于大数据的风控决策

## 1.2 HBase不适合的场景

- 跨机房部署
- Hadoop As a Service

## 1.3 阿里探索

- mysql 主从方案（HBase多集群）

### 1.4 Lindorm

-  高可用性（冗余副本）：Disk+Mem均带冗余
-  本地存储问题：热点数据，热点节点的备份
-  扩容的代价：采用分布式文件系统（盘古）
-  设计
    - 一个Region拥有多副本
    - 副本分布在不同的机房
    - 内存+buffer，消除GC
    - Java Map重写（SkipListMap）==> CCSMap
    - GC优化24倍
    - 读写速度20倍
    - 冷热分层异构存储（SSD + HDD），业务透明、自动分层、节省存储
    - LSM Tree的compaction

# 2. raft介绍

## 2.1 Raft协议
- Leader Election
- Log Replication
- Membership change
- Log compaction

- Raft复制结构
     - 树形复制
     - 多数复制
     - 写时复制
     - 断点续传
     - 两阶段SnapShot

- 高性能
   - Append log batch
   - Duplicate batch and pipline
   - Cache Last LogEntiries
   - Apply Async and Batch
     
## 2.2 基于Raft的存储模型
- 元信息管理
   - 容器系统Master
   - 流Master
   - 虚拟机Master
- 存储系统
   - 强一致Mysql
### 2.2.1 存储模型
   - Interface： Table，Block，Queue
   - Distrbuted： Shard，Placement，Rebalance
   - Engine：Table
   - Storage：Memory，SATA









