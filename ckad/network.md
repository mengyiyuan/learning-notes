## Network Policies
By default, all pods in the cluster can communication with any other pod, and reach out to any available IP.

NetworkPolicies limits what network traffic is allowed to and from pods in the cluster.

To get cluster IP of a pod:
`kubectl get pod <pod_name> -o wide`

From/To selectors specify which traffic sources and destinations are allowed by the rule. Types of selectors:
- podSelector: matches traffic from/to pods which match the selector
- namespaceSelector: matches traffic from/to pods within namespaces which match the selector.
- ipBlock: specifies a cidr range of IPs that will match the rule. Mostly used for traffic from/to outside the cluster. You can also specify exceptions to the range using `except`.