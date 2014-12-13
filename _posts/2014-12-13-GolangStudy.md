---
date: 2014-12-13 18:50
layout: post
title: Go学习笔记(1)
categories: Program
tags: golang
---

### What is Golang

Before I wrote this article, google released the Golang 1.4, so I will start the study of the go language from version 1.4.

The golang website is [golang.org](https://www.golang.org). You can find the golang doc and source code in this website.

Below is copyed from the golang website :

> Go is an open source programming language that makes it easy to build simple, reliable, and efficient software. 


### Golang startup

First of all we need setup the golang compile enviroment. I will use the debian7.1 for the example.

	$> wget https://storage.googleapis.com/golang/go1.4.linux-amd64.tar.gz

unzip the file to /usr/local

	#> tar -C /usr/local -xzf go1.4.linux-amd64.tar.gz

Open the /etc/profile

	#> vim /etc/profile

Add the golang path to the end of profile

	export GOROOT=/usr/local/go
	export PATH=$PATH:$GOROOT/bin
	
Check the version

	$> source /etc/profile
	$> go version
	
If display below infomation, the golang is well done:
	
	go version go1.4 linux/amd64
	
And then you can goto the /usr/local/go/test run some test code for sure the golang works well

	$> cd /usr/local/go/test
	$> go run helloworld.go 
		hello, world
		
Now, the golang enviroment can work well, happy coding :)


