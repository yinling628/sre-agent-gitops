Kubernetes Troubleshooting Guide
Pod Pending
If a Pod is stuck in the Pending state, it means it cannot be scheduled onto a node.
To debug this, use kubectl describe pod <pod-name>.
Common reasons include:
Insufficient CPU or memory resources on nodes.
The pod requests a PersistentVolume that is not available or unbound.
The pod has a node affinity that cannot be satisfied.
Pod CrashLoopBackOff
This means the container in the Pod is starting and then crashing repeatedly.
Check the logs of the crashing container using kubectl logs <pod-name>.
If the pod was restarted, you can check the previous container's logs with kubectl logs <pod-name> --previous or -p.
This is often caused by application errors, misconfiguration, or liveness probes failing.
ImagePullBackOff
This error indicates that the Kubelet on a node is unable to pull the container image.
Common causes are:
Incorrect image name or tag.
The image does not exist in the repository.
The node does not have credentials to access a private repository.
Network issues preventing the node from reaching the image repository.
