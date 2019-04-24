# Understanding Docker And Its Ecosystem:

Welcome to the world of containerization, microservice architecture. Lets not give attentions to these words. Just consider welcome. 

Before learning any thing, I always ask set of questions to myself like, what is this ? Why is this ? How is this ? What makes it ? What is its history ? What is its scope ? How it can be helpful in current scenario ? 

Once I get all the answers of all these question, I make myself educated in basics of that thing. On that knowledge again questions can be formed and same process is repeated. 

I would like to do the same thing to understand the docker from the scratch. 

Lets Start :

Lets understand the docker with a scenario. You started a company. You got a team of 2 dev, 1 qa , and 1 ops guy. At initial phase devs are working on the coding a monolithic application. Consider this is a simple application of hotel booking. 1 dev is working on front end , other is working on back end. When phase I is done , they decided to release the initial version. Question here is how to get the code to QA’s system for testing. QA guys test all feature on testing environment. Since this is first product from the company so we have branch new testing env with no other stuff on that host.

Question: How to get code from dev’s laptop to Qa’s env ? 

Points to cover :

History of software supply chain 

 -&gt; What is software supply chain 

 -&gt; How software used to be shipped to tester and then to production in old times 

 -&gt; What were the problems while shipping 

 -&gt; app1 got deployed and running in production after a long release cycle. 

 -&gt; app2 is ready to be released but app2 uses other version of libraries , framework. This app2 is planned to installed 

     on same prod host. Problems with dependencies. 

 -&gt; Analogy of container while shipping goods 

 -&gt; Same analogy is used while shipping software and this is done by docker. 

 -&gt; initial approach of promoting software using VM and its problems

 -&gt; So docker is a company which releases its product which is uses in software supply chain. 

 -&gt; Process in docker container 

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

————

Chapter 2. Playing around docker 

To understand the thing deeply it is best to play around with it. To understand the program it is best to write a code and make mistakes. Likewise to understand the docker lets play around it and dig it at every step.

You will have to install docker on the system first. This is not that difficult to do. You can follow docker installation guide from the official docker website. 

\( Ref: [https://docs.docker.com/v17.12/install/](https://docs.docker.com/v17.12/install/) \) 

Docker container run on the linux. Windows also provided the support for this also. To get a VM for docker to practice , we will use docker-machine. When you install docker on your local machine , docker toolbox will get installed. This tool is important for developers. Docker-Machine will create and manage the linux vm where docker is installed. In order to get VM, virtual box should be installed on your local machine. Install virtual box from the official website. 

docker-machine command output

Screenshot 1 

Create a vm using docker-machine command 

Screenshot 2 

Now you can see a vm named  is created in virtualbox 

Provide env to this vm and ssh to this vm using docker-machine command 

— 

Chapter 3 : 

Docker Images

Linux is built upon filesystem. That means everything is in the form of files and folders in linux. Whole operating system is basically a filesystem with files and folders stored on local disc storage. Docker is well known to be working on linux. That means the building block for container is in the form of files. Eventually docker image is the building block for docker container . 

Docker image is nothing but a layered file system. Each layer is stacked upon base layer. Each layer contains files and folder. This is to mention that docker uses unified file system. 

Docker images are just templates which drives the operations on container. Image is not a monolithic filesystem but it is divided into layer and each layer has files and folder. Layer is not mutable. No one can edit this layer which is the selling point of this docker. 

Lets explore the docker image. What is inside the docker image. 

To understand layering , pull a base image from docker hub registry. 

After running the command docker pull what exactly happened ?

Docker-base-image snapshot 01 

Docker Stores all filesystem somewhere in the local system, you can find that information by inspecting the pulled docker image 

Docker-base-snap02 

Cd to that /mnt/sda1/var/lib/docker/overlay2 

You can see a folder named “l”, my system shows one more but don’t worry I pulled same image twice that why it is showing new revision. We will come to this point later. 

lets Dig again and cd to that folder 

Docker-base-03 

Wow, what is inside this folder , isn’t that looks similar to unix filesystem ? 

Docker-base04

Yes , that unix filesystem from alpine flavor. This means when docker pulls the image , docker keeps filesystem of that base image in a deep location. This is nothing but keeping files and folders in short. 

We will see how is that get into action when containers is spun up. For now just understand how image is stored. 

If you try to make changes here in this filestrucure then consider that you broke everything . When docker image is pulled , it creates a symlink pointing to superlayer from the image. For every layer a symlink will be created pointing to the layer. 

Here till now we understood what exactly is docker image ? In which format image can be represented on local host. 

Docker-image-layer 

So what happens when container is spun up from this image. 

Image before spin 

If you see the folder structure before spinning the container . Symlink is pointing to the raw folder structure. 

When docker engine spins a container from the image, it add an extra writable container layer upon all the layers when image layers have been loaded into the memory. 

Preface: 

TODO

## 

