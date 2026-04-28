1.
For a battery-powered ESP32 temperature sensor that updates every 10 seconds, the most suitable QoS level is QoS 0 (Fire and Forget). This is because QoS 0 requires only a single packet transmission with no acknowledgment from the broker, resulting in the lowest possible power consumption and minimal bandwidth usage. Since temperature data is sent frequently, losing an occasional packet is acceptable because a new reading will be sent shortly after.

If QoS 2 is incorrectly selected, the system will require four packets per transmission (PUBLISH, PUBREC, PUBREL, PUBCOMP). This increases network overhead by four times compared to QoS 0. Mathematically, this means:

Battery usage increases significantly because the ESP32 must transmit and receive more packets.
Bandwidth consumption increases, which may lead to network congestion.
Over time, this results in reduced battery life and inefficient system performance, especially for devices that operate continuously.

2.
The fundamental difference between standard MQTT message delivery and a Retained message is that a normal MQTT message is only delivered to subscribers that are actively connected at the time of publishing. In contrast, a retained message is stored by the broker and automatically delivered to any new subscriber when it connects to the topic.

A real-world Industrial IoT (IIoT) scenario where a retained message is mandatory is in a factory safety system, such as a machine status indicator. For example, if a machine is in an “EMERGENCY STOP” state, this status must be immediately known by any monitoring system that connects later. Without the retained message, a newly connected client would not know the machine’s current state until the next update is published, which could lead to dangerous situations. By using the retained flag, the broker ensures that the latest critical status is always available instantly, improving system safety and reliability.
