# Falco-filebeat-daemonset

Daemonset configurations to get falco output scraped and sent by filebeat.

This exists to provide a concrete example for getting falco and filebeat working together. I took [falcosecurity's example daemonset](https://github.com/falcosecurity/falco/tree/dev/integrations/k8s-using-daemonset) and added the necessary filebeat components.

## Run
#### Configure the RBAC, Namespace, etc
```
:; kubectl create -f falco-rbac.yml
```

#### Create ConfigMap to store Falco & Filebeat configurations
```
:; kubectl create configmap --namespace security-system falco-config --from-file=falco-config
:; kubectl create configmap --namespace security-system falcobeat-config --from-file=falcobeat-config
```

#### Deploy the daemonset
```
:; kubectl create -f falco-daemonset-configmap.yml
```


## Verify
#### Find the pod && Peek the logs
```
:; kubectl get pods -A
:; kubectl --namespace security-system logs falco-daemonset-${RANDOM} filebeat
```

## Notes

The configurations are examples/templates. You'll want to change the output of your `falcobeat.yml` as well as tune Falco's rules in `falco-config`.
