# PoC RiseML cluster with GPU support

## requirements
* [packer](https://www.packer.io/) only if you need to build the AMI image 1.1.2
* [terraform](https://www.terraform.io/) 0.10
* [kubectl](https://github.com/kubernetes/kubectl) 1.8.4
* [Kubernetes Operations (kops)](https://github.com/kubernetes/kops) 1.8.0
* jq
* [riseml CLI](http://docs.riseml.com/install/cli.html) version 0.8.1

# instructions
* build the AMI if needed: https://github.com/beeva-mariorodriguez/beevalabs-docker-nvidia-ami tag: poc-riseml (currently 602636675831/beevalabs-docker-nvidia-jessie-0.6)
* build the k8s cluster: https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws branch: poc-riseml, follow instructions in [README.md](https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws/blob/poc-riseml/README.md)
* install RiseML: https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws branch: poc-riseml, follow instructions in [RISEML.md](https://github.com/beeva-mariorodriguez/kubernetes-gpu-aws/blob/poc-riseml/RISEML.md)
* good luck!

# notes

* careful with riseml CLI version, the install process always install the latest riseml and may not be compatible with your riseml cli (so: always update riseml CLI before installing the cluster!)
* project file documentation in http://docs.riseml.com is not very accurate
* also, ``riseml init`` creates an _invalid_ riseml.yml

# AWS AMI compatibility chart

## no GPU

| ami                                                           | k8s 1.8 | k8s 1.7 |
| ------------------------------------------------------------- | ------- | ------- |
| aws-marketplace/CoreOS-stable-1520.8.0-hvm-*                  |   :D    |   :D    |
| 900152524948/k8s-1.7-debian-jessie-amd64-hvm-ebs-2017-10-26   |   :(    |   ??    |
| 383156758163/k8s-1.7-debian-jessie-amd64-hvm-ebs-2017-12-02   |   :(    |   :(    |
| 383156758163/k8s-1.8-debian-jessie-amd64-hvm-ebs-2017-12-02   |   :D    |   ??    |
| 383156758163/k8s-1.8-debian-stretch-amd64-hvm-ebs-2017-12-02  |   :D    |   ??    |
| 602636675831/beevalabs-k8s-nvidia-jessie-0.7                  |   :D    |   ??    |

## GPU

| ami                                                           | k8s 1.8 |
| ------------------------------------------------------------- | ------- |
| 602636675831/beevalabs-k8s-nvidia-jessie-0.7                  |    :D   |

