# data-deploy


- Restart docker service
- run k8s locally with docker http://kubernetes.io/v1.0/docs/getting-started-guides/docker.html
- Create the services (rabbit, influxdb) and ReplicationControllers
    - kubectl create -f <name>-svc.yml
    - kubectl create -f <name>-rc.yml
- Create the pods for components (data-collector)
    - kubectl create -f <name>-svc.yml


# Useful

- updating an rc: kubectl rolling-update <rc-name> --file | --image
- check logs: kubectl logs <name>
- Stop the cluster
    - kill first the kubelet container
    - kill the rest: sudo docker ps --format="{{.ID}}"| awk '{print $1}' | xargs sudo docker kill
- etcd cluster healt: ./etcdctl -C 10.0.0.11:2379,10.0.0.12:2379,10.0.0.13:2379 cluster-health
# Check

- mount a volume to the kube node from the host machine
- monitoring http://kubernetes.io/v1.0/docs/user-guide/monitoring.html
- logging

