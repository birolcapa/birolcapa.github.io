---
id: 229
title: Simple Pcap Reader
date: 2018-10-01T20:45:17+00:00
author: birol
layout: post
guid: http://www.birolcapa.com/blog/?p=229
permalink: /2018/10/01/simple-pcap-reader/
categories: software
---
Pcap is a binary file which stands for packet capture file format.

Wireshark, the world’s foremost and widely-used network protocol analyzer, uses pcap file format to save captured network traffic.

According to the capture scenario, sometimes I need to search a special frame inside the pcap files. Since these pcap files have thousands of frames, searching a special frame is a time consuming task.

Therefore, I created a simple pcap reader which makes my life easy.

Here is the regarding source code with example application:

<a href="https://gitlab.com/birolcapa/simple-pcap-reader">https://gitlab.com/birolcapa/simple-pcap-reader</a>