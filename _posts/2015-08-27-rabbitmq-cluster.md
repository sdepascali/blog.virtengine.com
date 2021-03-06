---
title: Clustering RabbitMQ
layout: post
og_image_url: "https://blog.virtengine.com/res/gotalk-intro.png"
description: Clustering RabbitMQ
---

RabbitMQ is an open source message broker software (sometimes called message-oriented middleware) that implements the Advanced Message Queuing Protocol (AMQP). The RabbitMQ server is written in the Erlang programming language and is built on the Open Telecom Platform framework for clustering and failover.

RabbitMQ clustering simply involves configuring multiple slaves to connect to a master. When RabbitMQ is installed it works as an independent server (and that’s just fine, but clustering is recommended for production systems that want High Availability). You need to designate one server as a master, configure it, and then configure slaves to connect to this master. You can read a more thorough clustering guide on their site : https://www.rabbitmq.com/clustering.html. In this post I’ve condensed it down to the essentials.

###### My cluster setup

Each server is having ubuntu-14.04

	Each server's hostname should be unique
    Each server's erlang cookie should be unique

rabbitmq-master 192.168.1.11
rabbitmq-slave 192.168.1.12

###### Master setup
	IPADDRESS is 192.168.1.11
    Hostname is rabbitmq-master

1. install rabbitmq-server package

		$ sudo apt-get install rabbitmq-server

2. Edit `/etc/hosts` and add the entry for slave server

		192.168.1.12 rabbitmq-slave
3.
RabbitMQ is built on Erlang. Erlang systems which need to talk to each other must have the same magic cookie. This is a basic security mechanism to prevent unauthorized access to an Erlang system. An Erlang cookie is simply a hash stashed away in a file.
Copy the file content of

		/var/lib/rabbitmq/.erlang.cookie


###### Slave setup
	IPADDRESS is 192.168.1.12
    Hostname is rabbitmq-slave

1. install rabbitmq-server package

		$ sudo apt-get install rabbitmq-server

2. Edit `/etc/hosts` and add the entry for slave server

		192.168.1.11 rabbitmq-master
3. stop rabbitmq service

   	 $ sudo service rabbitmq-server stop

4. Edit the file content of `/var/lib/rabbitmq/.erlang.cookie`

		Paste the content you copied from master server

5. Create a file
`$ nano /etc/rabbitmq/rabbitmq.config`

		%%%
		%% Generated by Chef
		%%%
		[
	    {kernel, [
 	    ]},
      	{rabbit, [
	    {cluster_nodes, ['rabbit@rabbitmq-master','rabbit@rabbitmq-slave']},
        {tcp_listen_options, [binary, {packet,raw},
                                  {reuseaddr,true},
                                  {backlog,128},
                                  {nodelay,true},
     	{exit_on_close,false},
                                  {keepalive,false}]},
    	{default_user, <<"guest">>},
	    {default_pass, <<"guest">>}
	    ]}
		].

6. Start rabbitmq

		$ sudo service rabbitmq-server start

7. Start the cluster

		$ sudo rabbitmqctl stop_app
        $ sudo rabbitmqctl reset
        $ sudo rabbitmqctl start_app

 Taht's it. Now you can check your cluster status

 		$ sudo rabbitmqctl cluster_status

		Cluster status of node 'rabbit@rabbitmq-slave' ...
		[{nodes,[{disc,['rabbit@rabbitmq-master', 'rabbit@rabbitmq-slave']}]},
	    {running_nodes,['rabbit@rabbitmq-master', 'rabbit@rabbitmq-slave']},
		{partitions,[]}]
		...done.
