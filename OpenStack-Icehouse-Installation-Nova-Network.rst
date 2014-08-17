####
OpenStack Icehouse Installation: Two-node architecture with legacy networking (nova-network)
####

Welcome to OpenStack Icehouse installation manual !

This document is based on `the OpenStack Official Documentation <http://docs.openstack.org/icehouse/install-guide/install/apt/content/index.html>`_ for Icehouse. 

:Version: 1.0
:Authors: Chaima Ghribi and Marouen Mechtri
:License: Apache License Version 2.0
:Keywords: OpenStack, Icehouse, Two-node architecture, nova-network, Ubuntu 14.04


===============================

**Authors:**

Copyright (C) `Chaima Ghribi <https://www.linkedin.com/profile/view?id=53659267&trk=nav_responsive_tab_profile>`_

Copyright (C) `Marouen Mechtri <https://www.linkedin.com/in/mechtri>`_


================================

.. contents::



1. Basic Architecture & Network Configuration
=============================================

This document provides instructions on how to install and configure OpenStack icehouse on Ubuntu 14.04.
Here we consider a two-node architecture with legacy networking. It's a simple and easily deployable architecture that requires two node types:  

+ **Controller Node** that runs management services (keystone, Horizon…) needed for OpenStack to function.

+ **Compute Node** that runs the virtual machine instances in OpenStack. 

We have deployed a single compute node (see the Figure below) but you can simply add more compute nodes, if needed.  



.. image:: https://raw.githubusercontent.com/ChaimaGhribi/OpenStack-Icehouse-Installation-Nova-Network/master/images/two-node-topo.jpg



Unlike our `previous manual <https://github.com/ChaimaGhribi/OpenStack-Icehouse-Installation>`_, in which we considered a multi-node architecture with Openstack Networking (Neutron), this manual
details how to deploy OpenStack using a flat networking model. 

You need to create two networks:


+ **Management Network** (10.0.0.0/24): A network segment used for administration, not accessible to the public Internet.

+ **VM Traffic & External Network** (192.168.100.0/24): This network is connected to the controller node so users can access the OpenStack interfaces.
It is also used as internal network for traffic between virtual machines.


In the next subsections, we describe how to configure and test the network architecture. We want to make sure everything is ok before install ;)

So, let’s prepare the nodes for OpenStack installation!

1.1. Configure Controller node
------------------------------

* Change to super user mode::

    sudo su

* Set the hostname::

    vi /etc/hostname
    controller


* Edit /etc/hosts::

    vi /etc/hosts
        
    #controller
    10.0.0.11       controller   
       
    # compute1  
    10.0.0.31       compute1
    
    

* Edit network settings to configure interfaces eth0 and eth1::

    vi /etc/network/interfaces
    
    # The external network interface    
    auto eth0
    iface eth0 inet static
      address 192.168.100.11
      netmask 255.255.255.0
      gateway 192.168.100.1


    # The management network interface
    auto eth1
    iface eth1 inet static
      address 10.0.0.11
      netmask 255.255.255.0

* Restart network::

    ifdown eth0 && ifup eth0
    ifdown eth1 && ifup eth1
    

1.2. Configure Compute node
---------------------------

* Change to super user mode::

    sudo su

* Set the hostname::

    vi /etc/hostname
    compute1


* Edit /etc/hosts::

    vi /etc/hosts
    
    # compute1
    10.0.0.31       compute1
  
    #controller
    10.0.0.11       controller
    
    
* Edit network settings to configure interfaces eth0 and eth1::    
  
      vi /etc/network/interfaces
      
      # The external network interface    
      auto eth0
      iface eth0 inet static
        address 192.168.100.31
        netmask 255.255.255.0
        gateway 192.168.100.1

      # The management network interface
      auto eth1
      iface eth1 inet static
        address 10.0.0.31
        netmask 255.255.255.0

              
* Restart network::

    ifdown eth0 && ifup eth0
    ifdown eth1 && ifup eth1        

1.3. Verify connectivity
------------------------

    
* From the controller node::

    # ping the management interface on the compute node:
    ping compute1
    
    
* From the compute node::

    # ping the management interface on the controller node:
    ping controller    
    
