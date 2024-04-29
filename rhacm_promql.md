Curated list of useful promql queries when analyzing Multi-Cluster-Kubernetes issues
(could be a blog, could be shared with support)


1. **CPU Usage Across Clusters**:
```promql
sum(rate(container_cpu_usage_seconds_total{cluster=~"cluster.*"}[1m])) by (cluster)
```

2. **Memory Usage Across Clusters**:
```promql
sum(container_memory_usage_bytes{cluster=~"cluster.*"}) by (cluster)
```

3. **Network Traffic Across Clusters**:
```promql
sum(rate(container_network_transmit_bytes_total{cluster=~"cluster.*"}[1m]) + rate(container_network_receive_bytes_total{cluster=~"cluster.*"}[1m])) by (cluster)
```

4. **Pods Restarting Frequently**:
```promql
max by (cluster, namespace, pod) (kube_pod_container_status_restarts_total{cluster=~"cluster.*"} > 5)
```

5. **Node Disk Usage**:
```promql
node_filesystem_size_bytes - node_filesystem_free_bytes
```

6. **Pods Running per Namespace**:
```promql
count(kube_pod_info{cluster=~"cluster.*"}) by (namespace)
```

7. **Top CPU Consuming Pods**:
```promql
topk(5, sum(rate(container_cpu_usage_seconds_total{cluster=~"cluster.*", pod=~".*"}[5m]))) by (pod)
```

8. **Top Memory Consuming Pods**:
```promql
topk(5, sum(container_memory_working_set_bytes{cluster=~"cluster.*", pod=~".*"})) by (pod)
```

9. **Pods Pending Scheduling**:
```promql
count(kube_pod_status_phase{cluster=~"cluster.*", phase="Pending"})
```

10. **Node CPU Usage**:
```promql
sum(rate(node_cpu_seconds_total{cluster=~"cluster.*"}[1m]))
```

11. **Node Memory Usage**:
```promql
sum(node_memory_MemTotal_bytes) - sum(node_memory_MemFree_bytes) - sum(node_memory_Buffers_bytes) - sum(node_memory_Cached_bytes)
```

12. **Pods Evicted by Cluster**:
```promql
sum(kube_pod_container_status_reason{reason="Evicted",cluster=~"cluster.*"})
```

13. **Pod Restart Rate by Namespace**:
```promql
rate(kube_pod_container_status_restarts_total{cluster=~"cluster.*"}[5m]) by (namespace)
```

14. **API Server Request Rate**:
```promql
rate(apiserver_request_total{cluster=~"cluster.*"}[5m])
```

15. **API Server Latency**:
```promql
histogram_quantile(0.99, sum(rate(apiserver_request_duration_seconds_bucket{cluster=~"cluster.*"}[5m])) by (le))
```

16. **Node Disk Pressure**:
```promql
kube_node_status_condition{condition="DiskPressure",status="true",cluster=~"cluster.*"}
```

17. **Pods with High Network Traffic**:
```promql
sum(rate(container_network_receive_bytes_total{cluster=~"cluster.*",pod=~".*"}[1m]) + rate(container_network_transmit_bytes_total{cluster=~"cluster.*",pod=~".*"}[1m]))
```

18. **Node CPU Load Average**:
```promql
node_load1{cluster=~"cluster.*"}
```

19. **Pods Pending for Resource Constraints**:
```promql
sum(kube_pod_status_unschedulable{cluster=~"cluster.*"})
```

20. **Kubelet CPU Usage**:
```promql
rate(kubelet_runtime_operations_duration_seconds_count{cluster=~"cluster.*",component="kubelet"}[5m])
```
