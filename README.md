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

# Allowing cors

Basically, need to modify args passed to api server.
To do this, we need to modify the docker image, more specifically the file /etc/kubernetes/manifest/master.json where
the pods commands are defined.
Since the image has no editors, is easier to just mount a volume, copy it there to edit from the host and the copy back
in the image:

- docker run -it -v /tmp/:/tmp/files gcr.io/google_containers/hyperkube:<tag> /bin/bash
- Add "--cors-allowed-origins=.*" to apiserver command
- docker commit -m "Added cors" <image_id> gcr.io/google_containers/hyperkube:<tag>

Dockerfile and manifests come from kubernetes/cluster/images/hyperkube/ at github
