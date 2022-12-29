# Challenges in production

Kubernetes can be easy to install, but its complex to operate and maintain. Implmenting it in production brings challenges and difficulties.

In some cases, it must be made to interact with external infrastructure parts, or must be maintained between on-prem and cloud services, or the team implmenting it isn't quite knowledgeable.

production-readiness is the goal to achieve for this book. This readiness has a checklist:

- cluster infrastructure:
  - must run a highly avaialable control plane: run the control plane components on 3 or more nodes. Deploy Kuberenetes master components and the `etcd` on two separate node groups. Avoiding deploying pods to control plane nodes.
  - run a highly available workers group: achieved by running 3 or more worker instances. You should deploy them within an auto-scaling group and in different availability zones if possible. Also need to use the kubernetes cluster auto scaler, which enables worker nodes to horizontally upscale and downscale based on the cluster utilization.
- 
