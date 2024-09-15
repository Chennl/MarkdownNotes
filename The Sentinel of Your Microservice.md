# <img src="The Sentinel of Your Microservice/43697219-3cb4ef3a-9975-11e8-9a9c-73f4f537442d.png" alt="Sentinel Logo" style="zoom:50%;" />

# Sentinel: The Sentinel of Your Microservice

------

## Sentinel功能

------

- 流量控制：

  从系统稳定性角度考虑，虽然任意时间到来的请求往往是随机不可控的，而系统的处理能力是有限的，系统需要对没问题进行控制。 Sentinel 作为一个调配器，可以把随机的请求调整成合适的形状，如下图：

  ![arch](The Sentinel of Your Microservice/sentinel-flow-overview.jpg)

  流量控制有以下几个角度:

  - 资源的调用关系，例如资源的调用链路，资源和资源之间的关系；
  - 运行指标，例如 QPS、线程池、系统负载等；
  - 控制的效果，例如直接限流、冷启动、排队等。

- 熔断降级

​	什么是熔断降级: 由于调用关系的复杂性，如果调用链路中的某个资源出现了不稳定，最终会导致请求发生堆积。当调用链路中某个资源出现不稳定，例如，表现为 timeout，异常比例升高的时候，则对这个资源的调用进行限制，并让请求快速失败，避免影响到其它的资源，最终产生雪崩的效果。

 Sentinel 对这个问题采取了两种手段:

- 通过并发线程数进行限制

和资源池隔离的方法不同，Sentinel 通过限制资源并发线程的数量，来减少不稳定资源对其它资源的影响。这样不但没有线程切换的损耗，也不需要您预先分配线程池的大小。当某个资源出现不稳定的情况下，例如响应时间变长，对资源的直接影响就是会造成线程数的逐步堆积。当线程数在特定资源上堆积到一定的数量之后，对该资源的新请求就会被拒绝。堆积的线程完成任务后才开始继续接收请求。

- 通过响应时间对资源进行降级

除了对并发线程数进行控制以外，Sentinel 还可以通过响应时间来快速降级不稳定的资源。当依赖的资源出现响应时间过长后，所有对该资源的访问都会被直接拒绝，直到过了指定的时间窗口之后才重新恢复。

### 系统负载保护

Sentinel 同时提供[系统维度的自适应保护能力](https://sentinelguard.io/zh-cn/docs/system-adaptive-protection.html)。防止雪崩，是系统防护中重要的一环。当系统负载较高的时候，如果还持续让请求进入，可能会导致系统崩溃，无法响应。

在集群环境下，网络负载均衡会把本应这台机器承载的流量转发到其它的机器上去。如果这个时候其它的机器也处在一个边缘状态的时候，这个增加的流量就会导致这台机器也崩溃，最后导致整个集群不可用。

针对这个情况，Sentinel 提供了对应的保护机制，让系统的入口流量和系统的负载达到一个平衡，保证系统在能力范围之内处理最多的请求。

## Sentinel 是如何工作的

Sentinel 的主要工作机制如下：

- 对主流框架提供适配或者显示的 API，来定义需要保护的资源，并提供设施对资源进行实时统计和调用链路分析。
- 根据预设的规则，结合对资源的实时统计信息，对流量进行控制。同时，Sentinel 提供开放的接口，方便您定义及改变规则。
- Sentinel 提供实时的监控系统，方便您快速了解目前系统的状态。

## 流控降级与容错标准

Sentinel 社区正在将流量治理相关标准抽出到 [OpenSergo spec](https://opensergo.io/zh-cn/) 中，Sentinel 作为流量治理标准实现。有关 Sentinel 流控降级与容错 spec 的最新进展，请参考 [opensergo-specification](https://github.com/opensergo/opensergo-specification/blob/main/specification/zh-Hans/fault-tolerance.md)，也欢迎社区一起来完善标准与实现。

![opensergo-and-sentinel](The Sentinel of Your Microservice/opensergo-sentinel-fault-tolerance-zh-cn.jpg)
