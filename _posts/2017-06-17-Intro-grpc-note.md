---
layout: post
title: "Intro to gRPC 笔记"
date: 2017-06-17 22:11
tags:
    - gRPC
    - web
categories:
    - RPC
    - web
comments: true
description: "Intro to gRPC: A Modern Toolkit for Microservice Communication"
---

## From

视频来自[YouTube](https://www.youtube.com/watch?v=RoXT_Rkg8LA)

## REST

REST是web API的一个主要的结构（HTTP + JSON）

### 优点：

1. Easy to understand (text protocol)

2. Web infrastructure already built on top of HTTP

3. Great tooling for testing, inspection, modification

4. Loos coupling between clients/server makes changes(relatively) easy

5. High-quality http implementations in every language

### 缺点：

1. No formal(machine-readable) API contract (Need humans)

2. Streaming is difficult

3. Bi-directional streaming isn't possible at all

4. Operations are difficult to model (e.g. 'restart the machine')

5. Inefficient (textual representations aren't optional for networks)

6. Your internal services aren't RESTful anyways, they're just HTTP endpoints

7. Hard to get many resources in a single request

> gRPC fix all above.

类似于一个formal definition，意味着可以编译成任何语言

## gRPC: On the wire

1. HTTP/2

2. protobuf serialization (pluggable)

3. Clients open one long-lived connection to a gRPC server

    - A new HTTP/2 stream for each RPC call

    - Allows simultaneous in-flight RPC calls

4. Allows client-side and server-side streaming

## gRPC implementations

- C core

    - for Ruby, Python, node.js, PHP, C#, Objective-C, C++

    - PHP via PECL extension

- JAVA: Netty + BoringSSL via JNI

- Go: Pure Go implementation using Go stdlib cryptlo/tls package

## gRPC 缺点

1. Load balancing

2. Error handling is really bad

3. No support for browser JS

4. Breaking API changes

5. Poor documentation for some languages

6. No standardization across languages
