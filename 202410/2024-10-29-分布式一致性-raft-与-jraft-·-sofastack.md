# 分布式一致性 Raft 与 JRaft · SOFAStack
- URL: https://www.sofastack.tech/projects/sofa-jraft/consistency-raft-jraft/
- Added At: 2024-10-29 06:10:52
- [Link To Text](2024-10-29-分布式一致性-raft-与-jraft-·-sofastack_raw.md)

## TL;DR
分布式共识算法是多个参与者对某事达成一致的过程，Raft是其中一种算法，易于理解且模块化。Raft通过限制角色状态和简化设计实现共识，包含Leader选举、日志复制等功能。JRaft是基于Raft的Java实现，支持多Raft分组，具备性能优化和故障注入测试框架。JRaft设计包括节点、存储、状态机、复制和RPC等组件，实现了高效的线性一致读。

## Summary
# 分布式共识算法

## 理解分布式共识
- **多个参与者**：针对某一件事达成完全一致。
- **不可推翻**：已达成一致的结论。

## 分布式共识算法
- **Paxos**：分布式共识算法的根本，实现复杂。
- **Zab**：应用于Zookeeper，没有抽象成通用library。
- **Raft**：易于理解，业界实现众多。

## 什么是Raft？
- **核心协议**：基于Paxos精髓，模块化拆分和简化设计。
- **模块化拆分**：包括Leader选举、MemberShip变更、日志复制、Snapshot等。
- **简化设计**：限制角色状态、仅Leader可写入、随机化超时时间设计Leader Election等。

## 特点：Strong Leader
1. **唯一Leader**：同一时刻只能有一个Leader。
2. **Leader责任**：接收client请求，发送提案给followers，收集应答，发送心跳维持领导地位。

## 复制状态机
- **WAL**：将操作转化为write-ahead-log，保持WAL一致性。

## Raft中的基本概念
### Raft-node的3种角色/状态
1. **Follower**：完全被动，接受请求。
2. **Leader**：处理客户端请求，复制log到followers。
3. **Candidate**：竞选新leader。

### Message的3种类型
1. **RequestVote RPC**：发送投票请求。
2. **AppendEntries (Heartbeat) RPC**：复制日志条目，用作Heartbeat。
3. **InstallSnapshot RPC**：快照传输。

### 任期逻辑时钟
- **任期**：时间划分为任期，每个任期开始进行leader选举。

## Raft功能分解
### Leader选举
- **超时驱动**：Heartbeat/Election timeout。
- **随机超时时间**：降低选举碰撞概率。
- **选举流程**：Follower -> Candidate -> Leader或Follower。
- **新Leader选取原则**：最大提交原则。

### 日志复制
- **Raft日志格式**：`(TermId, LogIndex, LogValue)`。
- **连续性**：日志不允许出现空洞。
- **有效性**：相同term和logIndex的日志value相同。

### Commit Index推进
- **CommitIndex**：已达成多数派，可以应用到状态机的日志位置。

### AppendEntries RPC
- **完整信息**：日志信息、日志有效性检查、最新的提交日志位点。

## 什么是JRaft？
- **JRaft**：基于RAFT一致性算法的Java实现，支持MULTI-RAFT-GROUP。

## JRaft功能&性能优化
### 功能支持
- **Leader election**：Leader选举。
- **Log replication and recovery**：日志复制和恢复。
- **Snapshot and log compaction**：定时生成snapshot，实现log compaction。
- **Membership change**：集群配置变更。
- **Transfer leader**：主动变更leader。
- **网络分区容忍性**：对称和非对称网络分区容忍性。
- **容错性**：少数派故障不影响系统整体可用性。
- **Metrics**：内置性能指标统计。
- **Jepsen**：分布式验证和故障注入测试框架。

### 性能优化
- **Batch**：批量提交task、批量网络发送、本地IO batch写入、批量应用到状态机。
- **Replication pipeline**：流水线复制，降低数据同步延迟。
- **Append log in parallel**：并行持久化log entries和发送log entries。
- **Fully concurrent replication**：完全并发复制。
- **Asynchronous**：完全异步的callback编程模型。
- **ReadIndex**：优化raft read性能问题。
- **Lease Read**：通过租约保证leader身份，省去ReadIndex的heartbeat确认步骤。

## JRaft设计
- **Node**：Raft分组中的一个节点。
- **存储**：Log存储、Metadata存储、Snapshot存储。
- **状态机**：StateMachine、FSMCaller。
- **复制**：Replicator、ReplicatorGroup。
- **RPC**：RPC Server、RPC Client。
- **KV Store**：嵌入式分布式KV存储实现。

## JRaft Group
- **三副本架构**：单个节点的JRaft-node组成的三副本架构。

## JRaft Multi Group
- **多Raft group**：解决大流量读写瓶颈。

## JRaft实现细节解析之高效的线性一致读
- **线性一致读**：在t1时刻写入的值，在t
