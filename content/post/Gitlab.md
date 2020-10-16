---
title: "Gitlab"
date: 2016-04-15T09:41:39
categories: ["DevOps"]
tags: ["gitlab"]
keywords: []
author: "Canux"
draft: false
---

# Gitlab

gitlab是开源的有web界面的git服务器．

<https://about.gitlab.com/>

安装gitlab:

    sudo apt-get install -y curl openssh-server ca-certificates
    sudo apt-get install -y postfix
    curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
    sudo EXTERNAL_URL="http://gitlab.example.com" apt-get install gitlab-ee

配置:

    /etc/gitlab/gitlab.rb

升级gitlab:

需要先升级到下个major版本的最后一个稳定版.

    current:   9.4.7
    patch: gitlab-ce=9.5.10
    patch: gitlab-ce_10.8.7
    patch: gitlab-ce=11.11.8
    patch: gitlab-ce=12.0.12
    target: 12.10.14

runner:

    #sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
    sudo chmod +x /usr/local/bin/gitlab-runner
    sudo gitlab-runner install --user=canux --working-directory=/home/canux/gitlab
    sudo gitlab-runner register
    sudo gitlab-runner start

***

# CLI

备份：

    > 修改备份路径：gitlab_rails['backup_path'] = "/var/opt/gitlab/backups"
    # gitlab-rake gitlab:backup:create

重新加载配置:

    # gitlab-ctl reconfigure
