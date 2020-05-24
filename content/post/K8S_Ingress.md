---
title: "Ingress"
date: 2020-01-04T20:03:39+08:00
categories: ["Container"]
tags: ["k8s"]
keywords: []
author: "Canux"
draft: false
---

# Ingress-Nginx

<https://github.com/kubernetes/ingress-nginx>

    // 部署
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/mandatory.yaml
    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/baremetal/service-nodeport.yaml

    // 验证部署
    $ kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx --watch

    // Detect installed version
    POD_NAMESPACE=ingress-nginx
    POD_NAME=$(kubectl get pods -n $POD_NAMESPACE -l app.kubernetes.io/name=ingress-nginx -o jsonpath='{.items[0].metadata.name}')
    $ kubectl exec -it $POD_NAME -n $POD_NAMESPACE -- /nginx-ingress-controller --version

# traefik
