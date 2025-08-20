# support-net-tools Rock (OCI container image)
A [Rock](https://documentation.ubuntu.com/rockcraft/stable/explanation/rocks/) with some network tools for troubleshooting connectiviy issues from a container. 
It contains the following Ubuntu packages:
- tcpdump
- iperf3
- curl
- wget
- net-tools
- bash
- ethtool
- traceroute
- iputils-tracepath
- iputils-ping
- bind9-dnsutils
- bind9-host
- vim-tiny
- jq
- iproute2
- tar
- gzip
- openssl
- netcat-openbsd

## How to build
1. Clone this repository
2. `sudo snap install rockcraft --classic`
3. `sudo rockcraft pack -v`

As a result, the image archive _support-net-tools_latest_amd64.rock_ will be created.

## How to run
When run as a non-privileged container/pod, the `tcpdump` will not work as it requires root previleges.
### With Podman
1. `sudo apt install podman`
2. `podman run -d --rm --name ntools --privileged oci-archive:support-net-tools_latest_amd64.rock`
3. `podman exec -ti nettools /bin/bash`

### With MicroK8s
1. `microk8s ctr image import --base-name docker.io/library/nettools support-net-tools_latest_amd64.rock`
2. `kubectl run --privileged --image=docker.io/library/nettools:latest --image-pull-policy IfNotPresent nettools`

or

3. Save the following content to the _nettools.yaml_:
```
apiVersion: v1
kind: Pod
metadata:
  name: nettools
spec:
  containers:
  - name: nettools
    image: docker.io/library/nettools:latest
    imagePullPolicy: IfNotPresent
    securityContext:
      privileged: true
```
4. `kubectl apply -f nettools.yaml`
5. `kubectl exec -ti nettools -- /bin/bash`

Start using the tools inside the container/pod.

__Note__: Run the `tcpdump` without privilege dropping: `tcpdump -Z root ...`


## Related Documents
- [Rockcraft documentation](https://documentation.ubuntu.com/rockcraft/stable/)
- [man page: Podman](https://manpages.ubuntu.com/manpages/jammy/man1/podman.1.html)
- [MicroK8s documentation](https://microk8s.io/docs)
