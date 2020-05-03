# Redis cluster

## Creating the cluster
After running this commands (in this order):

- `$ kubectl apply -f redis.condif.map.yml`
- `$ kubectl apply -f redis.stateful.set.yml`
- `$ kubectl apply -f redis.svc.yml`

Do not forget to actually build the cluster, in other words, to assign the right *role* (master / slave)
for the right pod.

```
$ kubectl exec -it redis-cluster-0 -- redis-cli --cluster create --cluster-replicas 1 $(kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 ')
```

This specific part `$(kubectl get pods -l app=redis-cluster -o jsonpath='{range.items[*]}{.status.podIP}:6379 ')`,
looks for all pods labeled `redis-cluster` and get their internal ip and then append a `:6379` at the end.

## Test it

You can check the role for a specific pod with the following command:
```
$ kubectl exec -it redis-cluster-x -- redis-cli role
```

For example `kubectl exec -it redis-cluster-1 -- redis-cli role`