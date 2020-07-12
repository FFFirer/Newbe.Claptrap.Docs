---
title: '事件溯源（Event Sourcing）'
metaTitle: '事件溯源（Event Sourcing）'
metaDescription: '事件溯源（Event Sourcing）'
---

事件溯源模式是一种软件设计思路。这种设计思路通常与传统的采用增删查改（CRUD）为主的系统设计思路相区别。CRUD 应用通常存在一些局限性：

1. 通常来说 CRUD 应用会采用直接操作数据存储的做法。这样的实现方式可能会由于对数据库优化不足而导致性能瓶颈，并且这种做法会较难实现应用伸缩。
2. 在特定的领域通常存在一些数据需要注意对并发问题进行处理，以防止数据更新的错误。这通常需要引入“锁”、“事务”等相关的技术来避免此类问题。但这样又有可能引发性能上的损失。
3. 除非增加额外的审计手段，否则通常来说数据的变更历史是不可追踪的。因为数据存储中通常保存的是数据最终的状态。

与 CRUD 做法对比，事件溯源则从设计上避免了上述描述的局限性。接下来围绕上文中提到的“转账”业务场景简述事件溯源的基础工作方式。

采用 CRUD 的方法实现“转账”。

![采用CRUD的方法实现“转账”](/images/20190226-006.gif)

采用事件溯源的方式实现“转账”。

![采用事件溯源的方法实现“转账”](/images/20190227-001.gif)

如上图所示，通过事件溯源模式将转账业务涉及的余额变动采用事件的方式进行存储。同样也实现了业务本身，而这样却带来了一些好处：

- 通过事件，可以还原出账号任何阶段的余额，这就一定程度实现了对账号余额的跟踪。
- 由于两个账号的事件是独立处理的。因此，两个账号的处理速度不会相互影响。例如，账号 B 的转入可能由于需要额外的处理，稍有延迟，但账号 A 仍然可以的转出。
- 可以通过订阅事件来做一些业务的异步处理。例如：更新数据库中的统计数据，发送短信通知等其他的一些异步操作。

当然引入事件溯源模式之后也就引入了事件溯源相关的一些技术问题。例如：事件所消耗的存储可能较为巨大；不得不应用最终一致性；事件具备不可变性，重构时可能较为困难等。相关的这些问题在一些文章中会有较为细致的说明。读者可以阅读后续的延伸阅读内容，进而进行了解与评估。

> 参考资料
>
> - [Event Sourcing Pattern](https://docs.microsoft.com/en-us/previous-versions/msp-n-p/dn589792%28v%3dpandp.10%29)
> - [Event Sourcing Pattern 中文译文](https://www.infoq.cn/article/event-sourcing)