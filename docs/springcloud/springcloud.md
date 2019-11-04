# SpringCloud 学习笔记

## SpringCloud简单了解

> springcloud 是一系列框架的有序集合,简化了分布式系统的基础设施的开发:服务发现注册,配置中心,消息总线,负载均衡,断路器,数据监控等

### springcloud的核心成员

- spring cloud Netflix其中包含如下框架:
  - Netflix Eureka: 服务中心,云端服务发现
  - Netflix Hystrix: 熔断器,容错管理工具,通过熔断机制控制服务和第三方库的节点,从而对延迟和故障提供强大的容错能力
  - Netflix Zuul: 在平台上提供动态路由,监控,弹性,安全等边缘服务的框架
  - Netflix Archaius: 配置管理API,提供动态类型化属性配置等功能,这样修改了配置文件后不需要重启服务器
- Spring Cloud Config:配置中心,配置管理工具包,集中化管理配置
- Spring Cloud Buds: 事件消息总线,用于在集群中传播状态变化,可与Spring Cloud Config联合实现热部署
- Spring Cloud for Cloud Foundry