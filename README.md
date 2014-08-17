OpenStack Icehouse Installation: Two-node architecture with legacy networking (nova-network)
============================================================================================


This document provides instructions on how to install and configure OpenStack icehouse on Ubuntu 14.04.
Here we consider a two-node architecture with legacy networking. It's a simple and easily deployable architecture that requires two node types:  

+ **Controller Node** that runs management services (keystone, Horizon…) needed for OpenStack to function.

+ **Compute Node** that runs the virtual machine instances in OpenStack. 

We have deployed a single compute node (see the Figure below) but you can simply add more compute nodes, if needed.  

![Network Configuration Example](https://raw.githubusercontent.com/ChaimaGhribi/OpenStack-Icehouse-Installation-Nova-Network/master/images/two-node-topo.jpg)

Unlike our [previous manual] (<https://github.com/ChaimaGhribi/OpenStack-Icehouse-Installation>), in which we considered a multi-node architecture with Openstack Networking (Neutron), this manual
details how to deploy OpenStack using a flat networking model. 