# SpringCloud 学习笔记

## SpringCloud简单了解

> springcloud 是一系列框架的有序集合,简化了分布式系统的基础设施的开发:服务发现注册,配置中心,消息总线,负载均衡,断路器,数据监控等

### springcloud的核心成员

- spring cloud Netflix其中包含如下框架:
  - Netflix Eureka: 服务中心,云端服务发现
  - Netflix Hystrix: 熔断器,容错管理工具,通过熔断机制控制服务和第三方库的节点,从而对延迟和故障提供强大的容错能力
  - Netflix Zuul: 在平台上提供动态路由,监控,弹性,安全等边缘服务的框架
  - Netflix Archaius: 