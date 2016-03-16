#OpenStack deployment for Basic Topology

##Topology
![Topology](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/topology.PNG)

```
NET1 : BLUE Subnet = 192.168.1.0/24
Host = BLUE2/CirrOS VM@192.168.1.3
DHCP = 192.168.1.2 (dnsmsq)
```

```
NET2 : RED
Subnet = 192.168.2.0/24 
Host = RED2/CirrOS VM@192.168.2.5 
DHCP = 192.168.2.2 (dnsmsq)
```

``` Router : VR1 ```
	  
## Synopsis


## Code Example
There isn't much code to look at. This is more of a sys-config guide.
If you have trouble understanding it, email me at : [shreyas.enug@gmail.com](shreyas.enug@gmail.com)

##Theory Background

[OpenNetwork](https://www.opennetworking.org/about/onf-overview)
[OpenStack] (https://opensource.com/resources/what-is-openstack)


## Motivation
Well, to virtualize networking, set up dynamic routers and make topologies out of thin air? Sign me up, this is super interesting. 


## Installation
First things first : Get OpenStack. What falvour you get it as is going to be up to you. [http://docs.openstack.org/developer/devstack/](http://docs.openstack.org/developer/devstack/)
Don't install it, just GET it.

###In case you are running CentOS.
When you VmWare into your VM, probably, the internet is not going to work. Make sure that your Network Settings for the VM is on `NAT`.
When you boot into the machine, run `ifconfig` and see the interfaces that are listed. You are probably not going to see `eth0` because `CentOS` names things differently.
Go to `/etc/sysconfig/network-scripts` and `ls` to find `ifcfg-<device>`. 
Vim inside it using `sudo vim ifcg-<device>` and see if the following is true. When you're in Vim , press `i` to be in the `insert` mode. 
```
DEVICE=<device_name>
BOOTPROTO=none
ONBOOT=yes
BROADCAST=10.0.1.255
NETWORK=10.0.1.0
NETMASK=255.255.255.0
IPADDR=10.0.1.27
USERCTL=no
```

Then make the following changes :
```
DEVICE=eth0
BOOTPROTO=dhcp
ONBOOT=yes 
```

Also, this is a nice place to mention : **OpenStack does NOT work with NetworkManager ON**
so, go ahead and add this little line as well : `NM_CONTROLLED=no` to the above edits.
After you make changes in press `Esc` key to get out of `insert` mode and then do this `:wq!` to commit the changes.

###Check to see if `NetworkManager` is working. 
`chkconfig --list NetworkManager`
Then do : 
`# service NetworkManager stop` </br>
`# chkconfig NetworkManager off`</br>
`# service network start`</br>
`# chkconfig network on`</br>

Now, you're ready to install OpenStack. There might be some issues along the way with `httpd` and `keystone` and `puppet` but you'll figure them out as you go along. 
If you don't you can always reach me at `shreyas.enug@gmail.com`

After successfully installing OpenStack, check if it's running : `# openstack-status` , you should get a list of stuff deployed and if it is `ACTIVE`.
The login and password will be at : `# Vim keystonerc_admin`. 
The `Neutron-dashboard` will be running on : `yourIP:5000/dashboard` 

##Results

![Overview](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/overview.PNG)
![Networks](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/networks.PNG)
![Hypervisors](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/Hypervisors.PNG)
![Instances](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/instances.PNG)

[Usage Report](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/Usage%20Report.csv)
[Overall Usage](https://github.com/shreyasgune/OpenStack-Topo-Deployment/blob/master/Overall%20Usage.csv)


## API Reference
OpenStack Documentation : [http://docs.openstack.org](http://docs.openstack.org)


## Tests
Sadly, I could not SSH into the VM's. Everything ran super slow on my VM that ran OpenStack, so a trickle down effect was in order. Really not stoked. I'll fiddle with it on a dedicated server machine.

## Contributors
If you want to contribute or add to it or make it better, more readable, go for it. Tweet me issues if you can  : [@shreyaslumos](https://www.twitter.com/shreyaslumos) 
Many thanks to [David Mahler](https://www.linkedin.com/in/davidmahler) for his inputs on this. Also, huge shout outs to [Assaf Muller](http://assafmuller.com) <-- blog is amazing.

## License
<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">OpenStack Deployment for basic Topology</span> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

