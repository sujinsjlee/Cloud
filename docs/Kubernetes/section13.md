# Section13 - Troubleshooting

### Application Failure
- Check Accessibility

```console
~# curl http://web-service-ip:node-port
```

- Check Service Status

```console
~# kubectl describe service web-service
```

- Check POD

```console
~# kubectl get pod
~# kubectl describe pod [POD NAME]
~# kubectl logs [POD NAME]
```

- Check Dependent Applications

- Check Node Status

```console
~# kubectl get nodes
```

- Check Controlplane Pods

```console
~# kubectl get pods -n kube-system
```

- Check Services

```console
~# service [SERVICE NAME] status
~# kubectl logs [SERVICE NAME] -n kube-system
```
