---
title: "Vault"
date: 2016-04-15T09:41:39+08:00
categories: ["DevOps"]
tags: ["vault"]
keywords: []
author: "Canux"
draft: false
---

# Vault

<https://github.com/hashicorp/vault>

# CLI

server:

    // 启动vault
    $ vault server -config=/etc/vault/config.hcl

init:

    // init生成keys和token.
    $ vault operator init

    // 通过keys  unseal
    $ vault operator unseal

    // 通过token seal
    $ vault operator seal

auth:

    // 查看auth
    $ vault auth list

secrets

    // 查看secrets engine
    $ vault secrets list

    // enable kv-2
    $ vault secrets enable -version=2 kv
    $ vault secrets enable kv-v2

    // disable kv-2
    $ vault secrets disable kv-v2

policy

    // 查看policy
    $ vault policy list

    // 创建policy
    $ vault policy write <my-policy> ./my-policy.hcl

# API

<https://www.vaultproject.io/api>

***

# auth methods

<https://www.vaultproject.io/docs/auth>

# secrets engine

<https://www.vaultproject.io/docs/secrets>


