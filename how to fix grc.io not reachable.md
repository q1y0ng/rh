
If you are inside GFW in China and you will not be able to connecto to google server grc.io. When kubernetes try to pull the pause container from grc.io, it will get error and then stop:

`Failed to pull image "gcr.io/google_containers/pause:0.8.0": image pull failed for gcr.io/google_containers/pause:0.8.0, this may be because there are no credentials on this request.  details: (API error (500):  v1 ping attempt failed with error: Get http://gcr.io/v1/_ping: dial tcp 74.125.23.82:80: i/o t`

Please take following steps to fix this issue:

1 manually pull pause container from io.docker

`docker pull docker.io/kubernetes/pause`

2 rename the images

`docker tag docker.io/kubernetes/pause gcr.io/google_containers/pause:0.8.0`

3.

Optiona A: Manually start kubelet service and add flag --pod_infra_container_image. The following is an example how to start kubelet service manually:

`/usr/bin/kubelet --logtostderr=true --v=0 --address=10.66.208.230 --api_servers=10.66.208.164:8080 --hostname_override=10.66.208.230 --allow_privileged=false --auth_path=/var/lib/kubelet/auth --pod_infra_container_image="docker.io/kubernetes/pause:latest"--pod_infra_container_image="docker.io/kubernetes/pause:latest"`

OR

Optiona B: Change the /etc/kubernetes/kubelet file and add `pod_infra_container_image` there so you don't need to manully start kublete server everytime:

`KUBELET_ARGS="--pod_infra_container_image=docker.io/kubernetes/pause:latest"`

