# Section 5. Observability
- [POD Cycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)

    - Like individual application containers, Pods are considered to be relatively ephemeral (rather than durable) entities. Pods are created, assigned a unique ID (UID), and scheduled to nodes where they remain until termination (according to restart policy) or deletion. 

    - A Pod's status : Pending, Running, Succeeded, Failed, Unknown

## POD  Status
- The pod status tells us where the pod is in its lifecycle.
- When a pod is first created,
    - it is in a **`pending`** state.
    - This is when the scheduler tries to figure out where to place the pod.
- Once all the containers in a pod starts, it goes into a **`running`** state

## Readiness Probe
- The ready conditions indicate that the application inside the pod is running and is ready to accept user traffic.

```yaml
containers:
  - readinessProbe:
      httpGet:
        path: /api/ready
        port: 8080
      initialDelaySeconds: 10
      periodSeconds: 5
      failureThreshold: 8
```

- There are some additional options as well.

- **initialDelaySeconds** : If you know that your application will take a minimum of say 10 seconds to warm up, you can add an additional delay to the probe.

- If you'd like to specify how often to probe, you can do that using the **period seconds** option. (Frequency)
    - The periodSeconds field specifies that the kubelet should perform a liveness probe every 5 seconds. 

- By default, if the application is not not ready
after three attempts, the probe will stop. 
- If you'd like to make more attempts, use the **failure threshold** option


```yaml
containers:
  - readinessProbe:
      tcpSocket:
        port: 3306
```

```yaml
containers:
  - readinessProbe:
      exec:
        command:
          - cat
          - /app/is_r
```

- Now when the container is created, Kubernetes does not immediately set the ready condition on the container to true.
- Instead, it performs a test to see if the API responds positively.
- Until then, the service does not forward any traffic to the pod as it sees that the pod is not **ready**.

- without the readiness probe configured correctly, the service would immediately start routing trafficto the new pod.

- That will result in service disruption to at least some of the users. Instead, if the pods were configured with the correct readiness probe, the service will continue to serve traffic only to the older pods and wait until the new pod is ready.

- Once ready, the traffic will be routed to the new pod as well, ensuring no users are affected.

## LivenessProbe
- A liveness probe can be configured on the container to periodically test whether the application within the container is actually healthy.

# 97. Jobs
- https://kubernetes.io/docs/concepts/workloads/controllers/job/

- We have large datasets that require multiple pods to process the data in parallel. We wanna make sure that all pods perform the task assigned to them successfully and then exit. So we need a manager that can create as many pods as we want to get a work done and ensure that work gets done successfully.

- But we have learned about replica sets that help us create multiple pods. While a replica set is used to make sure a specified number of pods are running at all times, a job is used to run a set of pods to perform a given task to completion.

```
k create -f job-definition.yaml
k get jobs
k logs [JOB NAME]
```

# 98. CronJobs
> **CronJob** is meant for performing regular scheduled actions such as backups, report generation, and so on. 

- https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

```
k get cronjob
```

# 161. Deployment Strategy - Blue Green
- Blue-green is a deployment strategy where we have the new version deployed alongside the old version.
- So the old version is called blue, and the new version is called green
- we switch traffic to the new version all at once.

- For example, from code perspective, we simply change the label on the service selector to v2.

# 162. Deployment Strategy - Canary
- https://kubernetes.io/docs/concepts/cluster-administration/manage-deployment/#canary-deployments

- https://github.com/dgkanatsios/CKAD-exercises/blob/main/c.pod_design.md#deployments

- In this strategy, we deploy the new version and route only a small percentage of traffic to it.

1. Route traffice to both version
2. Route a small percentage of traffice to Version2
    - For this, we create a common label.
3. Reduce the number of the pods in the canary deploy


- The traffic split is always going to be governed by the number of pods present in each deployment.
- that's why service meshes like Istio comes with better control. With Istio, you can define the exact percentage of traffic to be distributed between each deployment, and it's not really dependent upon the number of pods in a deployment.
