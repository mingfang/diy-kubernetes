# DIY Kubernetes
WARNING: These instructions must be run as ``root``, and will expose all standard Kubernetes ports and a few non-standard ports to potential security attacks.  Use at your own risk.

## Running one Master and many Nodes on different hosts.
Create a Kubernetes clusters.  Tested on Ubuntu baremetal, KVM, AWS EC2, and Digital Ocean.

### Requirements
- Ubuntu 18.04
- Docker

### Run the Master

1. ```git clone https://github.com/mingfang/docker-kubernetes-master```
2. ```cd docker-kubernetes-master```
3. ```./build```
4. ```./run```

The Master is now running

### Run the Nodes(one per host)
Note: Tested on Ubuntu 18.04.  Newer versions should work but not tested.

1. ```git clone https://github.com/mingfang/docker-kubernetes-node```
2. ```cd docker-kubernetes-node```
3. ```./build```
4. On the Master, run ```docker exec kmaster /bootstrap-tokens.sh``` to generate the keys needed.
5. On the Node, run the command printed by #4. Should look something like this ```KUBELET_TOKEN=s.oKCwIqfs7LGbIHJv666K9oFV PROXY_TOKEN=s.4TkiUcFsscWufhHUOzPjKgxn ./run <master-host>```

The Node is now running.  Repeat for every host that runs the Nodes.

### Verify
1. ```alias kubectl='docker run --rm -it --net=host kubernetes-master kubectl'``` on the Master host
2. ```kubectl get nodes```
3. You should see all the Nodes running
```
NAME            LABELS                                            STATUS
192.168.1.160   host=minux,kubernetes.io/hostname=192.168.1.160   Ready
192.168.1.162   host=vm2,kubernetes.io/hostname=192.168.1.162     Ready
192.168.1.163   host=vm3,kubernetes.io/hostname=192.168.1.163     Ready
192.168.1.164   host=vm4,kubernetes.io/hostname=192.168.1.164     Ready
192.168.1.168   host=vm1,kubernetes.io/hostname=192.168.1.168     Ready
```

## Master
[docker-kubernetes-master](https://github.com/mingfang/docker-kubernetes-master)

## Node
[docker-kubernetes-node](https://github.com/mingfang/docker-kubernetes-node)

## Terraform Plugin
[terraform-provider-k8s](https://github.com/mingfang/terraform-provider-k8s)
