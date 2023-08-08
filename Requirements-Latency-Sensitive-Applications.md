# Requirements for Latency-Sensitive Applications

### What is Latency?

The amount of time between the start and completion of any given task is known as *latency*. There are no such thing as zero latency systems. 

Any given system's ability to tolerate additional latency is related to its current latency levels. A system that has nano-second performance requirements will feel the effects of a millisecond delay, whilst programs that function in seconds may not feel the effect of even multiple hundred of milliseconds of latency. 

### Low Latency Applications

Applications that fall under this category are those that require the execution of their tasks within the quickest possible time frame. Above all else, reaction speed is the most important criteria when measuring the performance of these applications. 

The acceptable level of latency within a program isn't a linear requirement. An application may have differing latency requirements for each of its individual capabilities. For example, we can look towards High-Frequency Trading systems.

> In HFT systems, order execution systems require the fastest possible performance. When the system identifies a trading opportunity and decides to execute a trade, it must send the corresponding order to the exchange as quickly as possible to take advantage of the market conditions. Any delay in order execution could result in missing out on the intended trade or executing it at a less favorable price.
> 
> Consider the logging and monitoring components of the system. These are responsible for recording trading activities, tracking system health, and generating reports for compliance purposes. While these aspects are important, they do not directly impact the speed of trade execution. As a result, the latency requirements for logging and monitoring can be more relaxed compared to the order execution component.
