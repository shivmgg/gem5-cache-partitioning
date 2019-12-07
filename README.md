# gem5-cache-partitioning

Set-way associative cache: This scheme is a compromise between the direct and associative
schemes. Here, the cache is divided into sets of tags, and the set number is directly mapped
from the memory address.

The ”Tag” field identifies one of the 64 different memory lines in each of the 64 different
”Set” values. Since each cache set has room for only two lines at a time, the search for a match
is limited to those two lines (rather than the entire cache). If there’s a match, we have a hit and
the read or write can proceed immediately. Otherwise, there’s a miss and we need to replace
one of the two cache lines by this line before reading or writing into the cache. (The ”Word”
field again select one from among 16 addressable words inside the line.)

Based on the description outlined above, L2 cache partitioning was added to the
gem5-Aladdin simulator. Cache partitioning assigns a subset of cache ways to each core such
that a core is limited to its assigned subset of ways for allocating lines in the cache. Partitioning
is only implemented for LRU replacement therefore, the cache must be configured to use the LRU
tag in order for partitioning to be enabled. In order to find which CPU has requested to allocate
a line in L2, the code looks for the CPU names in MasterName. This way, the CPU names
in the Python script must be the same as the constant CPU strings defined in allocateBlock.
It is possible to make the code more flexible by passing the partitions configuration and CPU
MasterNames from the Python script to the C++ code and avoiding any hard coded constant.
I implemented the static-cache partitioning in both classic and ruby memory model.

The Ruby memory system model provides gem5 a flexible infrastructure capable of accurately simulating a wide variety of memory systems. In particular, Ruby supports a domain specific lan-
guage called SLICC (Specification Language for Implementing Cache Coherence) where one can define many different types of cache coherence protocols. Essentially SLICC defines the cache,
memory, and DMA controllers as individual per-memory-block state machines that together form the overall protocol. By defining the controller logic in a higher level language, SLICC
allows different protocols to incorporate the same underlining state transition mechanisms with minimal programmer effort. 
Within the Ruby memory system, all objects are connected to each other via MessageBuffers.    


We have surpassed the value of current curTicks in the code and want to pass the same to a dynamic controller. Now, this agent will be outside the simulator and called after ev-
ery fixed number of curticks. The value of the curticks can be checked using the program src/sim/simulate.cc in gem5-ALaddin repo. We ran a small experiment to call a function from a program file
(which is outside the simulator) and print its values during the simulation process. It worked perfectly.

Changes can be found here: dynamic-cache-partitioning-step1.patch
