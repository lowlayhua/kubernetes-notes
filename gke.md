# GKE best practices
- Set appropriate resource limits and requests.
- Set your container resources to use the same amount of memory for both requests and limits and a bigger or unbounded CPU limit.
- [autopilot](https://cloud.google.com/kubernetes-engine/docs/concepts/autopilot-resource-requests)

# Container Image
- Ensure your container image is lean to avoid scale-up latency. 
- Use [dive](https://github.com/wagoodman/dive) to find ways to shrink your image. 
- [Hadolint](https://hadolint.github.io/hadolint/)  to run best practice container images.


# Use GKE streaming for fast application startup. 
- https://cloud.google.com/kubernetes-engine/docs/how-to/image-streaming
  
# Meaningful readiness and liveliness probes
- https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-setting-up-health-checks-with-readiness-and-liveness-probes
  
### Readiness probes 
- a must for all containers.
- This helps load balancer to understand whether to send traffic or not. 

### Liveliness probes 
- this will help to trigger self healing as will know when to restart the pods.

# Fine-tune the HPA utilization target.
- The following equation is a simple and safe way to find a good CPU target: 

          (1 - BUFF)/(1 + PERC)

- BUFF is a safety buffer that you can set to avoid reaching 100% CPU.
- This variable is useful because reaching 100% CPU means that the latency of request processing is much higher than usual. 
- PERC is the percentage of traffic that you expect to grow in two or three minutes.

- For example, if you expect a growth of 30% in your requests and you want to avoid reaching 100% of CPU by defining a 10% safety buffer, your formula would look like this:

(1 - 0.1)/(1 + 0.3) = 0.69

# Enable tracing to understand choke points in the application

# Group or separate workloads as needed. 
- https://wdenniss.com/autopilot-workload-separation

#  Consider retries with exponential back off using ASM.
- https://istio.io/latest/docs/concepts/traffic-management/#retries
  
#  Capacity buffers to deal with spiky burst of traffic with the help of balloon pods
- https://wdenniss.com/gke-autopilot-spare-capacity
  
#  Enable VPA in recommendation mode 
- https://cloud.google.com/kubernetes-engine/docs/concepts/verticalpodautoscaler


