# 蚂蚁金服开源 SOFAJRaft 详解
- URL: https://www.sofastack.tech/blog/sofa-jraft-deep-dive/
- Added At: 2024-10-18 13:50:06
- [Link To Text](2024-10-18-蚂蚁金服开源-sofajraft-详解_raw.md)

## TL;DR
SOFAStack是蚂蚁金服研发的金融级云原生架构，SOFAJRaft是其基于Raft算法的高性能Java实现，支持多Raft组，适用于高负载低延迟场景。Raft算法通过强Leader模式、Leader选举和成员变更确保一致性。SOFAJRaft包含多个模块，具备成员管理、容错性等特性，并进行了性能优化，如并行append log和异步化。性能测试显示开启复制流水线和Client-Batching后性能提升。相关资源包括PPT、GitHub、Wiki和文章等。

## Summary
1. **SOFAStack介绍**：
   - SOFAStack是蚂蚁金服自主研发的金融级分布式架构，包含构建金融级云原生架构所需的组件，是金融场景下的最佳实践。

2. **SOFAJRaft概览**：
   - SOFAJRaft是基于Raft一致性算法的生产级高性能Java实现，支持MULTI-RAFT-GROUP，适用于高负载低延迟场景。
   - 项目GitHub地址：[https://github.com/sofastack/sofa-jraft](https://github.com/sofastack/sofa-jraft)。

3. **Raft共识算法**：
   - Raft是一种共识算法，确保多个参与者对某件事达成一致，且已达成的一致结论不可推翻。
   - Raft算法的特点包括强Leader模式、Leader选举和成员变更。

4. **Leader选举**：
   - 在Raft算法中，每个Term期间的首要任务是选举Leader。
   - Leader选举涉及Term的概念，每个Term期间进行Leader选举。

5. **Log复制**：
   - Log复制是Raft算法在复制状态机中的重要应用，确保所有Server上的Log在内容和顺序上完全一致。

6. **SOFAJRaft设计**：
   - SOFAJRaft支持MULTI-RAFT-GROUP，适用于多种应用场景。
   - 设计包括Node、Log存储、Replicator、状态机、Meta Storage和Snapshot等模块。

7. **SOFAJRaft特性**：
   - 包括成员管理、Transfer leader、容错性、多数派故障恢复和Metrics等。
   - 支持预投票和非对称网络分区处理。

8. **SOFAJRaft优化**：
   - 性能优化措施包括并行append log、并发复制、异步化、批量化、复制流水线和线性一致读。
   - 通过ReadIndex和LeaseRead优化提高读操作性能。

9. **性能测试**：
   - 在特定测试条件下，开启复制流水线和Client-Batching后，性能显著提升。

10. **相关资源**：
    - 提供了SOFAJRaft的PPT下载、GitHub地址、Wiki和详细介绍文章等资源。
