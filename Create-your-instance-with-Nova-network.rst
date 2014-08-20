####
Create your first instance with Nova network
####

=============================

**Authors:**

Copyright (C) Chaima Ghribi

Copyright (C) Marouen Mechtri

=============================

In this guide we will show you how to create an instance with Nova network in few steps.

You will need to create your image first. After creating the image, create the network to which the instance will connect.
Finally, launch your instance and associate a floating IP address to it! 

Below are the steps in details ;)

1. Create your image
======================

* Create a simple credential file::

    vi creds
    #Paste the following:
    export OS_TENANT_NAME=admin
    export OS_USERNAME=admin
    export OS_PASSWORD=admin_pass
    export OS_AUTH_URL="http://controller:5000/v2.0/"

* Upload the cirros cloud image::

    source creds
    glance image-create --name "cirros-0.3.2-x86_64" --is-public true \
    --container-format bare --disk-format qcow2 \
    --location http://cdn.download.cirros-cloud.net/0.3.2/cirros-0.3.2-x86_64-disk.img

* List Images::

    glance image-list
    

2. Create initial network 
==========================


* Create a private network::
    
    source creds

    #Create the private network:
    nova network-create private --bridge br100 --multi-host T  --dns1 8.8.8.8  --gateway 172.16.0.1 --fixed-range-v4 172.16.0.0/24



* Create a floatingIP::

    source creds
    nova-manage floating create --pool=nova --ip_range=192.168.100.100/28

* List floating ips::

   nova-manage floating list 
 


3. Launch your instance !
=========================

* Generate a key pair::
 
   ssh-keygen

* Add the public key::
    
    source creds
    nova keypair-add --pub-key ~/.ssh/id_rsa.pub key1

* Verify the public key is added::
    
    nova keypair-list


* Add rules to the default security group to access your instance remotely::

   # Permit ICMP (ping):
   nova secgroup-add-rule default icmp -1 -1 0.0.0.0/0

   # Permit secure shell (SSH) access:
   nova secgroup-add-rule default tcp 22 22 0.0.0.0/0
   
   
* Launch your instance::    
    
    nova boot --flavor m1.tiny --image cirros-0.3.2-x86_64 \
    --security-group default --key-name key1 instance1

* Note: To choose your instance parameters you can use these commands::    
   
    nova flavor-list   : --flavor m1.tiny
    nova image-list    : --image cirros-0.3.2-x86_64
    nova secgroup-list : --security-group default
    nova keypair-list  : --key-name key1 

* Check the status of your instance::

    nova list
  

* Create a floating IP address::

    nova floating-ip-create

* Associate the floating IP address with your instance::

    nova floating-ip-associate instance1 192.168.100.97

* Check the status of your floating IP address::

    nova list

* Check network connectivity::

    ping 192.168.100.97

    # ssh into your vm using its ip address:
    ssh cirros@192.168.100.97


That's it! You can enjoy your instance ;)

License
=======
Institut Mines Télécom - Télécom SudParis  

Copyright (C) 2014  Authors

Original Authors - Chaima Ghribi and Marouen Mechtri

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except 

in compliance with the License. You may obtain a copy of the License at::

    http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


Contacts
========

Chaima Ghribi: chaima.ghribi@it-sudparis.eu

Marouen Mechtri : marouen.mechtri@it-sudparis.eu
