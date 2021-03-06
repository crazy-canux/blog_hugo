---
title: "Kubectl"
date: 2020-01-10T20:58:01+08:00
categories: ["Container"]
tags: ["tag"]
keywords: []
author: "Canux"
draft: false
---

# kubectl

kubectl是kubernetes的管理工具.

<https://github.com/cloudnativelabs/kube-shell>

<https://github.com/jonmosco/kube-ps1>

<https://github.com/ahmetb/kubectx>

在master上通过kubectl命令管理集群.

# options:

    kubectl options # 查看所有命令可用选项

    --kubeconfig
    kubectl --kubeconfig=$HOME/.kube.config (default)

    -n/--namespace

# basic command

create:

    # 通过yaml或json文件创建资源
    $ kubectl create -f FILENAME [options]

    options:
    -f/--filename

    kubectl create secret tls kubernetes-dashboard-tls --key ca.key --cert ca.crt -n kubernetes-dashboard

delete:

    // 删除资源
    $ kubectl delete (-f FILENAME | -k DICT | TYPE [(NAME|-l label|--all)]) [optiions]

    options:
    -f/--filename
    --all  
    --all-namespaces
    --force

    $ kubectl delete pods --all
    $ kubectl delete pod <name>

expose:

    # 创建serivce:
    $ kubectl expose (-f FILENAME | TYPE NAME) [--port=port] [--protocol=TCP|UDP|SCTP] [--target-port=...] [--name=...] [--external-ip=...] [--type=type] [options]

    # 创建service，并且使用NodePort
    # nodeport可以使用任意节点的IP访问.
    $ kubectl expose deployment hello-world --type=NodePort --name=example-service

run:

    # 创建pod/container(docker run):
    $ kubectl run NAME --image=image [--env="key=value"] [--port=port] [--replicas=replicas] [--dry-run=bool] [--overrides=inline-json] [--command] -- [COMMAND] [args...] [options]

set:

    $ kubectl set SUBCOMMAND [options]

    // 更新镜像
    $ kubectl set image deployment.v1.apps/<deploy-name> <container-name>=<image:tag>

get:

    // 获取resource信息(docker ps)
    $ kubectl [-n <namespace>] get [resource] [flags] [options]

    options:
    -A/--all-namespaces
    -f/--filename
    -o--output json/yaml/json/wide/name/...
    -w/--watch
    --watch-only

    kubectl get nodes --show-labels

    kubectl get nodes/no # 获取node节点信息
    kubectl get namespace/ns # 获取namespace信息
    kubectl get componentstatuses/cs

    kubectl get all --all-namespaces
    kubectl -n kube-system get all 

    kubectl get pod/pods/po 
    kubectl get po -w/--watch
    kubectl get po -o wide

    kubectl get deployment/deployments/deploy

    kubectl get service/services/svc

explain:

<https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/>

    // 查看资源解析
    $ kubectl explain RESOURCE [options]

    kubectl explain deploy.spec

edit:

    $ kubectl edit

# advanced commands

apply:

    $ kubectl apply

    $ kubectl diff

patch

    $ kubectl patch

    // 删除terminating状态的resource
    $ kubectl patch  persistentvolumeclaim/storage -p '{"metadata":{"finalizers":null}}' -n <ns>

    $ kubectl replace

    $ kubectl wait

    $ kubectl convert

    $ kubectl kustomize

# deploy command

scale:

    $ kubectl scale

autoscale:

    $ kubectl autoscale

rollout:

    // 回滚
    $ kubectl rollout

    // 暂停/恢复/重启 deployment.
    kubectl rollout pause/resume/restart  deploy/katib-mysql -n kubeflow

rolling-update:

    $ kubectl rolling-update

# debug&troubleshoot

describe:

    // like docker inspect
    $ kubectl describe (-f FILENAME | TYPE ... | TYPE/NAME) [options]

    options:
    -A/--all-namespaces
    -f/--filename

    // 查看node信息
    $ kubectl describe nodes <name>

    //查看pod详细信息
    $ kubectl describe pods <name> 

    // 获取k8s-dashboard的token
    $ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')

logs:

    # like docker logs
    $ kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER] [options]

    options:
    --all-containers
    -f/--follow
    -p/--previous

    // 查看pod中指定container log
    kubectl logs -f <pod> -c <container> -n <ns>

exec:

    # like docker exec
    $ kubectl exec (POD | TYPE/NAME) [-c CONTAINER] [flags] -- COMMAND [args...] [options]

    options:
    -c/--container
    -i/--stdin
    -t/--tty

    $ kubectl exec -it <pod> -n <ns> -- /bin/bash

port-forward:

由于已知的限制，目前的端口转发仅适用于 TCP 协议.

只能通过master IP 访问。

    $ kubectl port-forward

    // 转发本地端口到deploy/rs/svc/pod
    $ kubectl port-forward svc/redis-service 6379:6379 -n redis

    $ kubectl port-forward --address 0.0.0.0 -n kubernetes-dashboard service/kubernetes-dashboard 8080:443

proxy:

    $ kubectl proxy

attach:

    $ kubectl attach

cp:

    $ kubectl cp

auth:

    $ kubectl auth

# cluster management

top:

    $ kubectl top

    $ kubectl cluster-info

    $ kubectl certificate

    $ kubectl cordon

    $ kubectl uncordon

    $ kubectl drain

    $ kubectl taint
    
# Settings Commands

label

    label          Update the labels on a resource

    kubectl label nodes <your-node-name> disktype=ssd


annotate:

    annotate       Update the annotations on a resource

completion:

    completion     Output shell completion code for the specified shell (bash or zsh)

## others

    api-resources  Print the supported API resources on the server.

    api-versions   Print the supported API versions on the server, in the form of "group/version".

config:

    // 参考kubectx
    config         Modify kubeconfig files.

plugin:

    // 参考krew
    plugin         Provides utilities for interacting with plugins.

    // 查看安装的plugin
    $ kubectl plugin list

version:

    version        Print the client and server version information.

***

# shell-auotcompletion

bash/zsh 自动补全工具.

<https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion>

    $ kubectl completion bash | sudo tee /etc/bash_completion.d/kubectl

# krew

安装kubectl plugin的工具

<https://github.com/kubernetes-sigs/krew>

    // 下载插件列表
    $ kubectl krew update

    // 查找插件
    $ kubectl krew search

    // 安装插件
    $ kubectl krew install <name>
    
    // 升级插件
    $ kubectl krew upgrade

    // 卸载插件
    $ kubectl krew uninstall <name>

# kubectx 

在cluster和namespace之间切换的命令行工具.

包括kubectx 和 kubedns

<https://github.com/ahmetb/kubectx>

通过krew安装，作为kubectl的plugin:

    $ kubectl krew install ctx
    $ kubectl krew install ns
