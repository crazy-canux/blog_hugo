---
title: "Monitoring"
date: 2018-04-05T21:47:54
categories: ["Storage"]
tags: ["monitoring"]
keywords: []
author: "Canux"
draft: false
---

# Monitoring

## diskio

IOPS: Input/Output Per Second. 每秒输入输出量，也叫TPS.

    SELECT non_negative_derivative(mean(reads),1s) as "read" FROM "diskio"
    SELECT non_negative_derivative(mean(writes),1s) as "write" FROM "diskio"

throughput:

    SELECT non_negative_derivative(mean(read_bytes),1s) as "read" FROM "diskio"
    SELECT non_negative_derivative(mean(write_bytes),1s) as "write" FROM "diskio"

Utilization:

    SELECT non_negative_derivative(last("io_time"),1ms)
    FROM "diskio"
    WHERE "name" =~ /^(v|s|)d(a|b|c|d)$/
    GROUP BY "host","name",time(_interval)

Queue length:

    SELECT non_negative_derivative(last("weighted_io_time",1ms))
    FROM "diskio"
    WHERE "name" =~ /^(v|s|)d(a|b|c|d)$/
    GROUP BY "host","name",time(_interval)

