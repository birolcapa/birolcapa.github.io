---
id: 63
title: Network Traffic Replayer – Colasoft Packet Player Automation Tool
date: 2017-01-26T13:53:46+00:00
author: birol
layout: post
guid: http://www.birolcapa.com/blog/?p=63
permalink: /2017/01/26/network-traffic-replayer-colasoft-packet-player-automation-tool/
categories: software
---
When I make a contribution to Wireshark, I need to test my implementation before I commit my changes.

One of my test cases is checking whether Wireshark captures and shows the packets correctly.

Therefore, I should create network traffic for this test case.
After some searching, I found a tool: <a href="http://www.colasoft.com/download/products/download_packet_player.php">Colasoft Packet Player</a>.

Colasoft Packet Player is a UI tool and works at Windows. (I think it is a Windows Form Application). These properties were good for me since I was doing my tests by hand at Windows.

When I thought that I should do these tests automatically, I searched a way of making this. (I know there are many Windows and Linux tools which work from command line). Since Colasoft Packet Player doesn’t have an API, I should find a way of UI automation. (By the way, I want to use my PC especially keyboard and mouse, while the tests are running).

And I learned following:  
– Understand Colasoft Packet Player UI by using Microsoft Spy. <a href="https://birolcapa.github.io/2017/01/22/ui-automation-tool-helper-microsoft-spy/">See my previous post for details.</a>  
– How to make Win32 API calls by using C#

As a result I have written following code for automating Colasoft Packet Player.

<a href="https://gitlab.com/birolcapa/network-traffic-replayer">https://gitlab.com/birolcapa/network-traffic-replayer</a>

Following GIF is generated while one of the Unit Tests is running.

![Colasoft Gif](/images/cpp_gif2.gif)