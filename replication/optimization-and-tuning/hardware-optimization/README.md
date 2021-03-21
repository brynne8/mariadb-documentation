# Hardware Optimization

Better hardware is one of the easiest ways to improve performance.

As a general rule of thumb, hardware should be improved in the following order:

## Memory

Memory is the most important factor as it allows you to adjust the [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables). More memory means larger key and table caches can be stored in memory so that disk access, an order of magnitude slower, is reduced.

Simply adding more memory may not result in drastic improvements if the server variables are not set to make use of the extra available memory.

Using more RAM slots on the motherboard increases the bus frequency, and there will be more latency between the RAM and the CPU. So, using the highest RAM size per slot is preferable.

## Disks

Fast disk access is critical, as ultimately it's where the data resides. The key figure is the disk seek time, a measurement of how fast the physical disk can move to access the data, so choose disks with as low a seek time as possible.

You can also add dedicated disks for temporary files and transaction logs.

## Fast Ethernet

## CPU

Although hardware bottlenecks often fall elsewhere, faster processors allow calculations to be performed more quickly, and the results sent back to the client more quickly. Besides processor speed, the processor's bus speed and cache size are also important factors to consider.