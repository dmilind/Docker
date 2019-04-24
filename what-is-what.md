# What is What ?

What is Docker ?

 -&gt; Docker company provided a tool which can help engineer to ship a software to testing and production. 

Why Docker is required ?

 -&gt; something called image is generated which is immutable , 

 -&gt; solved the problems like, low resource management, easy deployment, fast release cycle , release isolation, 

     blue-green deployment support etc 

Docker Architecture: 

 -&gt; Diagram showing docker host 

 -&gt; three layers —&gt; linux host with namespace, cgroups, layer capabilities and unified filesystem 

 -&gt; Container run time —&gt; containerd and runc 

 -&gt; docker engine —&gt; libcontainerd , libnetwork , graph and plugins. 

What is namespace ?

What is cgroups ?

What is unified file system ?

What is container runtime ?

Consider container runtime as a single entity which is responsible for all the actions taken on container. These actions would be pulling image from the registry, starting , stopping , killing the container. 

Container runtime consist of two components  runc and containerd 

Runc: This is a low level functionality of container runtime. Runc is a collection of all features which helps to interact with underline system features in order to get container in actions. Runc provides below features: 

* Full support for linux namespace including user namespace 

 Native support for all security features available in Linux: Selinux, Apparmor, seccomp, control groups, capability drop, pivot\_root, uid/gid dropping etc. If Linux can do it, runC can do it.

• Native support for live migration, with the help of the CRIU team at Parallels

• Native support of Windows 10 containers is being contributed directly by Microsoft engineers

• Planned native support for Arm, Power, Sparc with direct participation and support from Arm, Intel, Qualcomm, IBM, and the entire hardware manufacturers ecosystem.

• Planned native support for bleeding edge hardware features – DPDK, sr-iov, tpm, secure enclave, etc.

• Portable performance profiles, contributed by Google engineers based on their experience deploying containers in production.

• A formally specified configuration format, governed by the Open Container Project  under the auspices ofthe Linux Foundation. In other words: it’s a real standard.

\(Ref : [https://blog.docker.com/2015/06/runc/](https://blog.docker.com/2015/06/runc/)\) 

Containerd: This is high level functionality in container runtime. This is daemon service running in collaboration with runc. Container d is responsible for complete container lifecycle. It is helpful to pull images , initiate container from the image. Run the container, kill the container etc. 

