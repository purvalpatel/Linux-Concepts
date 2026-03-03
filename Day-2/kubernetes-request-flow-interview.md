Below is the user-request flow in kubernetes.
```
user --> DNS
Load balancers --> Node
Kube-proxy --> Service
Service --> Pod (Via Iptables)
Containers inside pod
```
1. How does the kube-proxy decide which pod gets traffic?
- It works with the Iptables or IPVS.
- Kube-proxy doesnt think.


2. What if Pod dies Mid-Requests?
- Client see the failure if request failed in between.
- Kubernetes not save the request.
- It doesnt route the half open connection.
 
3. How readliness probe affect traffic routing ?



4. Why does traffic sometimes goes to the unhealthy pods ?
- Kubernetes sends traffic only id pof is into endpoint list.
- If pod is in endpoint list it receives the traffic, if not in endpoint it will not.
- If "readlinessProbe" not set, traffic starts showing.
- ReadlinessProble must be set with the valid health check not with (/).
