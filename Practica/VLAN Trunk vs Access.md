### Access Port

The access port is a type of port that is like a type of connection on a switch that provides the virtual machines with connectivity via a switch or VLAN. Basically, it transmits data to and from a specific/single VLAN. This doesn’t cause signal issues because the frames remain within the same VLAN.  For complex networks, it is not that efficient to work with. One can configure the port as a host port, to optimize the performance of the access port.

**Advantages**

1. It sends and receives frames that aren’t tagged.
2. There are no signal issues in the access port as the frame remains in a single VLAN.
3. To decrease the time it takes the designated port to begin to forward packets, use the host port.

**Disadvantages**

1. It can carry traffic for only one VLAN.
2. Only an end station can be set as a host port.

### Trunk Port

A trunk port is a port, which is used to connect to another switch or router. It is a link that carries many signals simultaneously. Basically, it can transmit data from multiple VLANs. It uses tags in order to allow signals to get to the correct endpoint. Trunk Port offers higher bandwidth and lower latency. The Trunking takes place in layer 2 of the OSI model, which is known as the “data link layer”. The device uses the IEEE 802.1Q encapsulation or tagging method, in order to correctly deliver the traffic on a trunk port with several VLANs.

**Advantages**

1. It offers higher bandwidth and lower latency.
2. It sends all signals across a single trunk link which is for each switch or router.
3. It can carry traffic for several VLANs simultaneously. 

**Disadvantage**

1. It is quite complex to set up when compared with the Access port.