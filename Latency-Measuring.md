# How Latency is Measures

### TTFB

Time To First Byte. Measures the time it takes for a client's request to reach the server and then for the server to begin sending the first byte of data back.

### RTT

Round-Trip Time. Measures the time it takes for a packet of data to travel from the source to the destination and back again. 

### Application Response Time

This measures the time it takes for an application to respond to a user action, such as clicking a button or submitting a form. It includes the processing time on the server and any network delays.

### Jitter 

Jitter measures the variation in latency or delay between data packets in a network. It's commonly used in real-time communication applications to ensure consistent playback or display of audio and video.

### CPU clock cycles

A unit of time that is used to measure the operations and performance of the CPU in a computer or electronic device. It uses clock signals, execution of instructions, pipelines, CPI (cycles per instruction), and clock speed and performance to make its measurement. This type of measurement is most useful for measuring instruction level latency, engineers on HFT systems may commonly measure how many instructions are executed and how many CPU clock cycles were required. Besides kernel optimizations, this is the highest level of optimization. 

### Tick-to-trade 

Also called TTT, this is the time from when a market data packet takes to hit the trading server until the trading server is done processing the packet and sends a packet out to the trading exchange. This includes the time spent by the infrastructure reading the packet, processing the packet, calculating its trade signals, generating the order and putting the order on the wire.

## Latency Metrics

### Peak Latency

Peak latency measures the maximum delay or time it takes for a specific operation or request to complete. It represents the worst-case scenario in terms of delay.

### Mean Latency

Mean latency, also known as average latency, calculates the average time it takes for a set of operations or requests to complete. It provides an overall picture of typical delay.

### Latency Variance

Latency variance measures the degree of variation in latency times across a set of operations. It indicates how consistent or predictable the latency is.

### Throughput

Throughput measures the rate at which a system or process can handle and complete a certain number of operations or requests within a given time frame. It focuses on the system's capacity to handle tasks efficiently.
