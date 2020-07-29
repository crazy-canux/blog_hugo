---
title: "API"
date: 2020-01-10T20:55:52+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# API

api-server统一的操作入口.

kubectl, UI, 等都是通过api-server操作资源.

payload可以是json，也可以是yaml.

yaml文件中#表示行注释。

***

# yaml

部署k8s可以通过yaml文件来配置资源.

资源对象组成部分:

    apiVersion: 
    kind: 
    metadata: 元数据
    spec: 期望的状态
    status: 观测到的状态

查看apiVersion:

    kubectl api-versions

查看Kind:

    kubectl api-resources

metadata:

    metadata:

      name:
      namespace:

      labels/标签: 用户筛选资源，唯一的资源组合方法, 可以使用selector来查询.

      annotations/注解: 存储资源的非标识性信息，扩展资源的spec/status.

      ownerReference/关系: 方便反向查找创建资源的对象，方便进行级联删除。

spec:

status:

***

# Pod

pod

    apiVersion: v1
    kind: Pod
    metadata:
      name: test
    spec:
      securityContext: // pod级别security context定义
        runAsuser: 1000
        runAsGroup: 3000
        fsGroup: 2000

      initContainers: // 
      - name: init
        image: my-image
        command: ...
        args: ...

      containers:
      - name: test
        image: image
        resources:
          requests:  // 申明需要的资源
            memory: "64Mi"  // byte
            cpu: "250m"     // millicore (1 core = 1000 millicore)
            ephemeral-storage: "2Gi" // byte
          limits:
            memory: "128Mi"
            cpu: "500m"
            ephemeral-storage: "4Gi"
        command: ['/bin/bash', '-c', 'env']
        env:
        - name: special_level_key
          valueFrom: // 挂载ConfigMap
            configMapKeyRef:
              name: cm-name
              key: cm-key
        volumeMounts: // secret以文件形式挂载到/etc/foo
        - name: foo
          mountPath: "/etc/foo"
          readOnly: true

      volumes:
      - name: foo // 指定要挂载的secret
        secret:
          secretName: mysecret

      imagePullSecrets:  // 私有镜像授权
      - name: regcred

      nodeSelector: // 将pod部署到指定node
        key: value

pod中的container共享存储(pod volume):

    apiVersion: v1
    Kind: Pod
    medadata:
    spec:

      # 两种pod volume
      volumes:
      # emptyDir： pod删除之后该目录也会被删除
      - name: cache-volume
        emptyDir: {}
      # hostPath: pod删除之后该目录还在host上. 
      - name: hostpath-volume
        hostPath:
          path: /path/on/host

      containers:
      - name: container1
        image: test
        volumeMounts:
        - name: cache-volume
          mountpath: /path/on/container
          # subPath会在emptyDir或hostPath目录下创建子目录
          subPath: cache1
      - name: container2
        image: test
        volumeMounts:
        - name: hostpath-volume
          mountpath: /path/on/container
          readOnly: true

***

# Deployment

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my-app-deploy
      namespace: my-ns
      lables:
        app: my-app
    spec:
      replicas: 3
      # 选择器
      selector: 
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - name: my-app
            image: image:latest
            imagePullPolicy: IfNotPresent/Always
            ports:
            - containerPort: 443
            volumeMounts:
            - mountPath: /path/on/cpod
              name: my-volumes
          volumes:
          - name: my-volumes
            hostPath: 
              path: /path/on/host

***

# Job

Job

    appVersion: batch/v1
    kind: Job
    metadata:
      name: my-job
    spec:
      # 代表本pod队列执行此次数(被执行8次)
      completions: 8
      # 代表并行执行个数(同时有两个在运行)
      parallelism: 2
      backoffLimit: 4
      template:
        spec:
          containers:
          - name: my-job
            image: my-image
            conmand: ['test']
          restartPolicy: Never

***

# CronJob

CronJob

    apiVersion: batch/v1
    kind: CronJob
    metadata:
      name: my-cj
    spec:
      schedule: "*/1 * * * *"
      startingDeadlineSeconds: 10
      concurrencyPolicy: Allow
      successfulJobsHistoryLimit: 3
      jobTemplate:
        spec:
          template:
            spec:
              containers:
              - name: image
                image: image
                args:
                - /bin/sh
                - -c
                - date
              restartPolicy: OnFailure

***

# DaemonSet

DaemonSet

    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: my-ds
      namespace: my-ns
      labels:
        k8s-app: my-app
      spec:
        selector:
          matchLabels:
            name: my-app
        template:
          metadata:
            labels:
              name: my-app
          spec:
            containers:
            - name: my-container
              image: my-img

***

# ConfigMap

configmap的配置在pod中无法修改绑定的文件.

ConfigMap

    apiVersion: v1
    kind: ConfigMap
    metadata:
      labels:
        app: flanel
        tier: node
      name: flannel-cfg
      namespace: kube-system
    data:
      cni-conf.json: |
        {
          "name": "n1"
        }

创建配置文件的configmap

    // 创建cm,然后导出yaml即可release出去
    $ kubectl -n app create cm my-conf --from-file ./config.ini
    $ kubectl -n app get cm -o yaml 

***

# Secret

secret

    apiVersion: v1
    kind: Secret
    metadata:
      name: mysecret
      namespace: kube-system
    type: Opaque
    data:
      username: name 
      password: pw

创建私有镜像账号

<https://kubernetes.io/zh/docs/tasks/configure-pod-container/pull-image-private-registry/>

    // 创建secret然后导出到yaml，就可以release出去.
    $ kubectl -n app create secret docker-registry regcred --docker-server=https://harbor.domain.com --docker-username=user --docker-password=pw --docker-email=canuxcheng@gmail.com
    $ kubectl -n app get secret harbor-secret -o yaml 

***

# ServiceAccont

    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: default
      namespace: default
    data:
      namespace: ...
      ca.crt: ...
      token: ...
    type: kubernetes.io/service-account-token

    secrets: // 指定secret
    - name: default-token

    imagePullSecrets: // 使用该sa的都会自动授权registry.
    - name: myregistrykey

***

# PV

pv没有namespace

## static volume provisioning

    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: nas-csi-pv
    spec:
      capacity:
        storage: 5Gi
      accessModes:
      - ReadWriteMany
      persistentVolumeReclaimPolicy: Retain
      csi:
        driver: ...

## dynamic volume provisioning

***

# pvc

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: nsa-pvc
      namespace: test
    spec:
      accessModes:
      - ReadWriteMany
      resources:
        requests:
          storage: 5Gi 

    apiVersion: v1
    kind: Pod
    spec:
      volumes:
      - name: nas-pvc
        persistentVolumeClaim:
        claimName: nas-pvc 
      containers:
      - name: c1
        volumeMounts:
        - name: nas-pvc
          mountPath: /data  

***

# StatefulSet

StatefulSet 中的 Pod 拥有一个唯一的顺序索引和稳定的网络身份标识。

    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 2
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
      volumeClaimTemplates:
      - metadata:
          name: www
        spec:
          accessModes: [ "ReadWriteOnce" ]
          resources:
            requests:
              storage: 1Gi



