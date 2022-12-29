# Introducing Kubernetes

In the past, most applications were monolithic. Now, most monoliths are being broken into smaller, independently running components, called microservices.

microservices can be developed and deployed individually rather than as a collective. This becomes difficut to configure and manage to keep the whole system running smoothly.

Kubernetes enables developers to deploy their applications themselves and as often as desired, without requiring assistance from an operations team. it also enables operations teams to automatically monitor and reschedule deployments of applications in the event of a hardware failure. Systems Administrators also can shift from monitoring the applications individually to managing kubernetes and the rest of the infrastructure, while kubernetes takes care of the applications.

Kubernetes abastracts away the hardware infrastructure and exposes your whole data-center as a single enormnous computational resource; removing the need to know which servers are deploying which application specifically. This can be useful for on-prem apps, but is most used for running applications on cloud providers.

Kuberenetes enables developers to deploy with this simple platform, while not requiring the cloud providers system admins to know anything about the thousands of apps running on their hardware. It neables developers to configure and deploy their applications without any help from the system admins and allows the system admins to focus on keeping the underlying infrastructure up and running, while not having to know anything about the actual apps runnning on the infrastructure.

## Containers and kuberenetes

Kubernetes uses linux container technologies to provide isolation of running applications, like Docker containers. A container is a single isolated process running on a host operating system, consuming only the resources that the app consumes without the overhead of system level processes, which a virtual machine would have.

You should have one container for each service in the application.

The benefit of containers are that there is only the single kernel from the host OS running all the instructions, the CPU doesn't need to do any kind of virtualization the way it does with VMs.

The main benefit of virtual machines are the full isolation provided. Each VM runs its own Linux kernel, while the containers all call out to the same kernel, which can be a security risk.

The greater number of isolated processes you want to run, VMs become limiting because they have this system-level computations happening. In this situation, containers are a much better choice.

### Isolating with Containers

Two mechanism make isolation possible when running on the same operating system:

- Linux Namespaces: these make sure each process sees its own personal view of the system (files, processes, network interfaces, hostnames, and so on).
- Linux Control groups (cgroups): these limit the amount of resources the process can consume.

By default, each linux system has a single namespace. But then you can create additional namespaces and organize resources within them.

There are multiple types of namespaces that can be created:

- a mount namespace (mnt)
- a process ID (pid)
- Network (net)
- inter-process communication (ipc)
- Unix Time-Sharing (UTS)
- User ID (user)

Each namespace kind is used to isolate a certain group of resources.

For example, The network namespace determines which network interfaces the application running inside the process sees. Each container uses its own set of network interfaces.

**Note**: one caveat to how useful containers are is that they run on the hosts' linux kernel. So if the application services launched on a host require a different version of a linux kernel, the application will not run correctly.

Kubernetes works with other container engines, but Docker is the primary container engine. `rkt` is an alternative that's available and supported by Kubernetes.

## Kubernetes History

It started out as a Google internal product called Borg, which developed into Omega, and then was made available to the public as Kubernetes in 2014.

## Core of what Kubernetes is doing

Kubernetes enables you to run your software app on thousands of computer nodes as if all those nodes were a single, enormous computer.

Deploying apps through kubernetes is always the same whether your cluster contains only a couple of nodes or thousands. The size of the cluster doesn't matter at all.

The kubernetes system is composed of a master node and any number of worker nodes. A developer then submits apps to the master node, and Kubernetes is responsible for deploying the apps to the cluster of worker nodes. The node used for one of the apps should not matter, to either the developer or the system administrator.

The developer can specify if some apps should run together, and Kubernetes will deploy them to the same worker node.

Kubernetes, in this way, can be thought of as an operating system for the cluster. IT relieves the developers from having to implment certain infrastructure-related services into their applications directly, and they can instead rely on kubernetes for those services, such as:

- service discovery
- scaling
- load-balancing
- self-healing
- leader election

Kubernetes does all this while achieving better resource utilization than is possible with just manual scheduling of scaling and switching servers for applications.

### Kubernetes cluster structure

A cluster is composed of many nodes, which are either master nodes or worker nodes:

- master nodes host the kubernetes control plane that controls and manages the whole kubernetes system
- worker nodes run the application you deploy

The control plane is what controls the cluster and makes it function. This has multiple components:

- the kubernetes API server
- the scheduler
- the controller manager
- etcd: a reliable distributed data store that persistently stores the cluster configuration

These elements hold nad control the state of your cluster, but don't run the cluster.

The woker nodes are the machines the container images are launched on. These nodes provide these services with the following components:

- A container runtime (i.e. docker) that runs your containers
- Kubelet: which talks to the API server and manages containers on its node
- Kubernetes Service Proxy (kube-proxy) which load-balances the network traffic between application components.

## Running an Application

To run an application, you need to package up your application in one or more container images, push those images to an image registry, and then post a description of your app to the Kubernetes API server.

The description will include information about the container images, how those components are related to each other, and which ones need to be run co-located. This will also specify how many replicas of each node you want to run. It also defines whether components provide service to external or intrenal clients, and whether or not the services should be exposed through a single IP address and made discoverable by other components of the app.

Once the API server processes the description, the schedule schedules the specified groups of containers onto the available worker nodes based on available computational resoruces on each node.

The kubelet on those nodes then instructs the container runtime to pull the required container images and run the containers.

## Adminstrating an Application

once the app is running, Kubernetes continuously makes sure that the deployed state of the application always matches the description you provided. Kubernetes will restart instances that crash or stop responding automatically, and will select new nodes as needed if a worker node dies.

You can decide to increase or decrease the number of copies while the clusters are running, and kubernetes will spin up additional ones or stop excess ones as needed.

The container images needed for specific services can be stored as environment variable,s so that Kubernetes can access them when a worker node goes down.

## Summary of Kubernetes Benefits

- Simplifies application deployment and development
- achieves better utilization of hardware components
- has automatic health checks and is self-healing
- automates rescaling

**NOTE**: Kubernetes benefits developers because their application runs in the same environment for development as it does in production, which has a big effect on when bugs are discovered. Also, Kubernetes reduces the amount of application features for networking and routing, and state management that would otherwise have to be added into the service by the developer. Kubernetes can also stop a bad deployment from rolling out immediately.
