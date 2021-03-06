---
title: Installing VirtEngine on CentOS
layout: post
og_image_url: "https://blog.virtengine.com/images/intro-install-virtengine.png"
description: How to launch CouchDB in VirtEngine.com
---
## Introduction

![Install VirtEngine Virtualization Cloud Platform](https://blog.virtengine.com/images/intro-install-virtengine.png)

Before we begin, lets install OpenJDK8 and cassandra.

This tutorial will help you set up VirtEngine.

## Prerequisites

### OpenJDK8

> su -c "yum install java-1.8.0-openjdk"

### Cassandra 3.x 

[Cassandra Installation](http://docs.datastax.com/en/cassandra/3.x/cassandra/install/installRHEL.html)

### Install VirtEngine

> sudo yum update  
>  
> sudo yum install verticenilavu verticegateway verticensq vertice verticevnc

## Conclusion

These are the very simple steps to launch our platform in CentOS. Now you need to configuire it!