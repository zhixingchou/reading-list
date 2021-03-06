---
title: Google运维解密读书笔记
date: 2018-06-01 16:17:24
tags: [读书]
---

今天是儿童节呢。


不能将碰运气当成战略 --SRE俗语
系统运维本质上是人与计算机共同参与的一项系统性工程。

Dev与OPS分离两项成本
- 直接成本（人力成本、团队随业务规模呈线性增长）
- 间接成本（研发运维团队目标不同，沟通成本）



## 拥抱变化

SER职责
可用性改进、延迟优化、性能优化、效率优化、变更管理、监控、紧急事务处理、容量规划与管理

SRE团队的运维工作比重应该在50%以下，确保可以长期关注研发工作
ON-call准则：在8-12小时的on-call轮班期间最多只处理两个紧急事件，以保证on-call工程师有足够的时间跟进紧急事件。


Google准则：对事不对人，事后总结的目标是尽早的发现和堵住漏洞，而不是通过流程去绕过和掩盖它们。


错误预算：任何产品都不是也不应该做到100%可靠。根据产品特性制定合理的错误预算，从而尽可能的加快新功能上线速度。常见战术策略有灰度发布、1%AB测试等

监控系统不应该依赖人来分析报警信息，而是应该由系统自动分析，仅当需要用户执行某种操作时，才需要通知用户。一个监控系统应该只有三类输出
- 紧急报警（alert）：需要用户立即执行某种操作，从而解决某种已经发生的问题或即将发生的问题
- 工单（ticket）：系统不能自动解决目前的情况，用户应该执行某种操作，但并非需要立即执行
- 日志（logging）：平时没有人会去主动阅读日志，除非特殊需要比如调试及事后分析


评价运维团队将系统恢复到正常情况的有效指标就是MTTR（平均恢复时间），另外可靠性还包含另一个指标MTTF（平均失败时间）

变更管理：SRE的经验是大概70%的生产事故是由于某种部署变更而触发的，变更管理的最佳实践是通过自动化来完成以下几个项目
- 采用渐进式的发布机制
- 迅速而准确的检测到问题的发生
- 当出现问题时，安全迅速的回退改动


容量规划必须的几个步骤
- 必须有一个准确的自然增长需求预测模型(需求预测的时间应该超过资源获取的时间)
- 规划中必须有准确的非自然增长的需求来源统计
- 必须有周期性的压力测试，以便准确的将系统原始资源信息和业务容量对应起来

服务质量指标（SLI）
服务质量目标（SLO）
服务质量协议（SLA）

区别SLO和SLA的一个简单指标就是是否如果SLO没有达到时有什么后果，若果没有明确定义后果，那么我们讨论的就是一个SLO而不是SLA

软件工程的一个关键思想（不仅仅是运维部分）是保持简单


## 拥抱风险

事实证明构建一个100%可靠的服务是不必要的，极端的可靠性会带来成本的大幅提升，过分的追求稳定性会限制新功能的开发与交付。

度量服务的风险（基于时间的可用性、合计可用性）

服务的风险容忍度

## 服务的质量目标

并不是监控系统中所有的指标都要定义为SLI；需要理解用户对系统的真实需求才能真正决定哪些指标有用。指标过多会影响我们真正需要关注的指标。

SLO目标的选择原则
- 不要用目前的状态为基础来选择目标，要从全局出发，不能被当前的架构环境所束缚
- 保持简单
- 避免绝对值
- SLO越少越好
- 不要追求完美

## 减少琐事

SRE应该把更多的时间花费在长期项目研发上而非日常运维琐事中。
琐事是指手动性的、重复的、可以被自动化的、战术性的、没有持久价值的工作，而且琐事是与服务发展存在线性增长关系。

## 分布式系统监控

监控的目的
- 分析长期趋势
- 跨时间范围的比较
- 报警
- 构建监控台页面
- 临时性的回溯分析

监控系统应该解决两个问题：什么东西故障了以及为什么出现故障，区分这两点是构建信噪比高的监控系统最重要的概念。

4个黄金指标：延迟、流量、错误、饱和度（saturation）

度量监控指标的时候采用合适的精度（根据服务的特点、SLA需求进行采集）

简化监控系统
- 能反映真实故障的规则应该越简单越好，可预测性强
- 不常用的数据收集、汇总以及报警配置应该定期删除
- 收集的信息但是没有暴露给任何监控台或被任何报警规则使用的应该定期删除

当为监控系统和报警系统增加新规则时，可以考虑以下几点
- 该规则是否可以检测到以前检测不到的可能发生或即将发生用户可见故障
- 是否可以忽略这条报警？什么情况可能会导致用户忽略这条报警，如何避免？
- 这条报警是否确实显示了用户正在受到的影响？是否存在误报？测试或维护状态下这个报警是否应该被过滤


## Google自动化系统的演进

自动化的价值
- 一致性，手动操作必然存在人为的不一致性，这些不一致性会导致错误、疏漏、数据质量问题及可靠性问题
- 平台性，通过正确的设计和实现，自动化系统可以提供一个可以扩展的、广泛适用的、甚至可能带来额外收益的平台
- 修复速度快，自动化处理系统中的常见故障，可以降低平均修复时间（MTTR）
- 行动速度更快
- 节省时间

自动化的演进路径遵循以下几点
- 没有自动化
- 外部维护的系统特定的自动化系统
- 外部维护的通用的自动化系统
- 内部维护的系统特定的自动化
- 不需要任何自动化的系统

## 发布工程

发布工程（Release Engineering）是软件工程内部一个较新、发展较快的学科，这个学科主要专注于构建和交付。

发布工程哲学
- 自服务模型。发布工程师开发工具制定最佳实践，让产品研发团队可以自己掌控和执行自己的发布流程，每个团队必须能够自给自足。
- 追求速度。（可借鉴每小时构建基于测试和功能选择可用构建版本发布，也有测试就发布这种）
- 密闭性。基于同一源码的构建结果应该是相同的，构建过程使用指定版本的构建工具及依赖库，编译过程自包含，不依赖编译环境之外的其他服务。
- 强调策略和流程。多层安全和访问控制机制，确保发布过程中只有指定的人才可以执行指定的操作。

持续构建与部署

设置科学的代码分支策略来进行构建测试发布

发布工程的几个问题：
- 如何管理包版本
- 是采用持续部署模型还是定期构建，发布的频率如何
- 如何管理配置文件
- 哪些发布过程的指标有用

## 简单化

可靠性只有靠最大程度的简化不断追求而得到。 --C.A.R.Hoare, Turing Award lecture

软件系统本质上是动态和不稳定的。我们的工作最终就是再系统的灵活性和稳定性上维持平衡。

保持代码简单，定期清理源代码中的无用代码（注释及功能开关未启动的代码）

书写一个明确的、最小的API是管理软件系统简单性的必要部分。

## 具体实践

SRE职责：
- 低级：能够正常对外服务
- 高级：主动控制服务状态，而不是被动救火

## 基于时间序列数据进行有效报警

Riemann/Heka/Bosun/Prometheus都是开源软件中与Borgmon基于时间序列报警理念类似的系统。

报警设置的最小持续时间值一般至少被设置为两个计算周期，以确保偶尔一次的信息收集失败不会立即出发报警

## on-call轮值

on-call工程师负责处理生产环境中即将或者正在发生的业务事故以及评审对生产系统的变更请求，在on-call时工程师承诺可以再分钟级别响应，具体时间应该是SRE团队和产品团队共同商议得出。

可参考的on-call标准
- 对直接面向最终用户的服务或时间非常紧迫的服务，时间可定为5分钟
- 非敏感业务，时间可以定为30分钟

## 有效的故障排查手段

故障排除需要的两个条件
- 对通用故障排查过程的理解
- 对发生故障的系统足够了解

提高故障排查效率建议
- 增加可观察性。在实现之初就给每个组件增加监控指标和结构化日志
- 利用成熟的观察性好的组件接口设计系统

## 紧急事故管理

嵌套式职责分离（在事故处理中让每个人清楚自己的职责）
- 事故总控
- 事故处理团队（唯一对系统进行修改变更）
- 发言人&规划负责人（协助包括对外发言、事故文档、Bug报告等）

什么时候对外宣布事故（Google的标准）
- 是否需要引入第二个团队来帮助处理问题
- 是否正在影响最终用户
- 在集中分析一个小时后问题依然没有得到解决

事故流程管理最佳实践
- 划分优先级：控制影响范围，恢复服务，同时为根源调查保存现场
- 事前准备：事先就准备好完备的流程
- 信任：相信事故处理参与者分配职责后让其自主行动
- 反思：事故处理过程中注意自己的情绪和精神状态
- 考虑替代方案：周期性审查项目状况，考虑目前做的事是否正确是否有必要调整
- 练习：多使用制定好的流程使之成为习惯
- 换位思考：鼓励每个团队成员熟悉流程中的其他角色

## 事后总结：从失败中学习

基本的事故总结条件：
- 用户可见的宕机时间或服务质量降低到一定标准
- 任何类型的数据丢失
- on-call工程师需要人工介入的事故（包括rollback、切换用户流量等）
- 问题解决耗时超过一定限制
- 监控问题（预示着问题是由人工发现而非报警系统）

协作及知识共享
建立事后总结文化

## 跟踪故障

## 测试可靠性

如果你还没有亲自试过某件东西，那么就假设它是坏的

软件测试分为两大类：传统测试及生产测试

传统测试
- 单元测试：最小最简单的软件测试形式（如一个类、一个函数的正确性），也是用来保证某个函数或模块完全符合系统对其的行为要求的一种规范
- 集成测试：通过单元的测试的组件组装成大的系统组件，然后进行集成测试检验该组件功能的正确性
- 系统测试：包括（冒烟测试、性能测试、回归测试）

生产测试
- 配置测试
- 压力测试
- 金丝雀测试（结构化的最终用户验收测试）

## SRE部门中的软件工程实践

SRE开发的工具是一个完整的软件工程项目，而不是一次性的脚本和小补丁。开发这些工具的SRE也需要对内部用户的需求进行产品规划制定未来的发展方向。

容量规划：基于意图的容量规划

表达产品意图的先导条件
- 依赖关系
- 性能指标
- 优先级

向大型团队推广内部软件的要点：
- 持续和完善的推广方案
- 用户的用户
- 资深工程师和管理层的赞助让他们看到项目的实用性潜力

Google的一些指导经验
- 创建并宣扬一个明确的信息
- 评估组织的能力
- 发布并且迭代
- 不要降低标准

## 前端服务器的负载均衡

考虑的几个因素：
- 逻辑层面（全局还是局部）
- 技术层面（硬件和软件）
- 用户流量的天然属性

使用DNS进行负载均衡
虚拟IP

## 应对过载

避免过载是负载均衡策略的一个重要目标

QPS陷阱：Google的经验按照QPS来规划服务容量或者是按照某种静态属性一般是错误的，更好的解决方案是直接以可用资源来衡量可用容量，绝大多数情况下简单使用CPU数量作为资源配给主要信号就可以工作的很好，原因如下
- 在有垃圾回收（GC）机制的编程环境中，内存的压力通常自然而然的变成了CPU压力
- 在其他编程环境里，其他资源一般可以通过某种比例进行配置

## 处理连锁故障

连锁故障产生的原因：服务器过载、资源耗尽、服务不可用

资源耗尽
- CPU：请求数量上升、队列过长、线程卡主、CPU死锁或者请求卡主、RPC超时、CPU缓存效率下降
- 内存：任务崩溃、GC速率加快导致cpu使用率上升、缓存命中率
- 线程
- 文件描述符：不足可能会导致无法建立网络连接
- 资源之间的相互依赖

应对过载
- 使用负载均衡技术
- 提供降级结果
- 过载时主动拒绝请求
- 上层系统主动拒绝请求
- 进行容量规划

队列管理

流量抛弃和优雅降级
FIFO（先入先出）队列模式、LIFO（后入先出）
优雅降级（Graceful degradation）可在流量抛弃的基础上进一步减少服务器的工作量。

连锁故障的触发条件
- 进程崩溃
- 进程更新
- 新的发布
- 自然增长
- 计划中或计划外的不可用
- 资源限制

解决连锁故障的立即步骤
- 增加资源
- 停止健康检查导致的任务死亡
- 重启软件服务器
- 丢弃流量
- 进入降级模式
- 消除批处理负载

## 管理关键状态：利用分布式共识来提高可靠性

传统ACID数据存储语义（原子性、一致性、隔离性、持久性）
新的分布式存储语义（基本可用、软状态、最终一致性）

分布式共识常用协议（Paxos、Raft、Zab、Mencius）
很多成功使用分布式共识算法的系统常常是作为该算法实现的服务的一个客户端来使用的。如ZooKeeper、Consul、Etcd

## 数据完整性：读写一致

提供超高的数据完整性的策略
- 在线时间
- 延迟
- 规模
- 创新速度
- 隐私

区别备份与存档的重要区别就是数据恢复（没有人想备份数据他们只是想恢复数据），备份时可以直接被应用程序重新加载的。

数据完整性是手段，数据的可用性是目标
交付一个恢复系统，而非备份系统










