# PoC RiseML cluster with GPU support

## requirements
* [packer](https://www.packer.io/) only if you need to build the AMI image
* [terraform](https://www.terraform.io/) 
* [kubectl](https://github.com/kubernetes/kubectl)
* [Kubernetes Operations (kops)](https://github.com/kubernetes/kops)
* jq
* [riseml CLI](http://docs.riseml.com/install/cli.html) version 0.8.0

# instructions
* build the AMI if needed: https://github.com/beeva-mariorodriguez/beevalabs-docker-nvidia-ami tag: poc-riseml (currently 602636675831/beevalabs-docker-nvidia-jessie-0.6)
* build the k8s cluster: https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws branch: poc-riseml, follow instructions in [README.md](https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws/blob/poc-riseml/README.md)
* install RiseML: https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws branch: poc-riseml, follow instructions in [RISEML.md](https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws/blob/poc-riseml/RISEML.md)
* good luck!

# notes
* non GPU version (using CoreOS ami) works (almost) OK, the ``riseml system test`` sometimes fails:
    ```
    2.build | [2017-11-15T11:01:35Z] Building your image (localhost:31500/build:admin-smoke-test-555d99e-dxgir7zr)
    2.build | [2017-11-15T11:01:35Z] Downloading code (http://riseml-gitweb:8888/users/0b687763-0757-11e7-875b-80e65006b9ce/repositories/smoke-test?revision=555d99e842b553eeea8d9aaf72627a03730f4a55)
    2.build | [2017-11-15T11:01:35Z] Running install commands...
    2.build | [2017-11-15T11:01:35Z] --> RUNNING
    2.build | [2017-11-15T11:02:36Z] UnixHTTPConnectionPool(host='localhost', port=None): Read timed out. (read timeout=60)
    2       | [2017-11-15T11:02:37Z] --> FAILED
    2.build | [2017-11-15T11:02:37Z] --> FAILED
    2.build | [2017-11-15T11:02:37Z] Reason: ERROR
    2.build | [2017-11-15T11:02:37Z] Exit Code: 1
    ```
* GPU version has some problems:
    * nvidia driver setup is pretty manual: could not make nvidia-docker work reliably on jessie (more [here](https://github.com/beeva-mariorodriguez/beevalabs-docker-nvidia-ami/commit/a0b09d222d1fbe5443eba4e99c61f26a4e763736))
    * nvidia driver installation in stretch works better _but_ could not bootstrap kubernetes (1.8.x) using stretch (kube-dns errors)
    * anyway: ``riseml system test`` fails
* careful with riseml CLI version, the install process always install the latest riseml and may not be compatible with your riseml cli (so: always update riseml CLI before installing the cluster!)

# AWS AMI compatibility chart

## no GPU

| ami                                                           | k8s 1.8 | k8s 1.7 |
| ------------------------------------------------------------- | ------- | ------- |
| aws-marketplace/CoreOS-stable-1520.8.0-hvm-*                  |   :D    |   :D    |
| 900152524948/k8s-1.7-debian-jessie-amd64-hvm-ebs-2017-10-26   |   :(    |   ??    |
| 383156758163/k8s-1.7-debian-jessie-amd64-hvm-ebs-2017-12-02   |   :(    |   :(    |
| 383156758163/k8s-1.8-debian-jessie-amd64-hvm-ebs-2017-12-02   |   :D    |   ??    |
| 383156758163/k8s-1.8-debian-stretch-amd64-hvm-ebs-2017-12-02  |   :D    |   ??    |


