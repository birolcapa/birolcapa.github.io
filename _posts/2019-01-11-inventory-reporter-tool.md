---
id: 260
title: Inventory Reporter Tool
date: 2019-01-11T23:08:07+00:00
author: birol
layout: post
guid: http://www.birolcapa.com/blog/?p=260
permalink: /2019/01/11/inventory-reporter-tool/
categories: software
---
Software engineering teams use lots of hardware to develop new products. As a result, every colleague has minimum one Laptop or more like: one Desktop, multiple screens... Moreover all these machines have multiple test cards, network interface controllers, etc.

I recognized that most of the colleagues were suffering from tracking the inventory records.

The main problem was finding a hardware (e.g PCIe cards, RAMs, screens) in the team and this means excessive email traffic in the team. In addition, nobody knows who is responsible for which device. This situation was creating a strong friction for both of development and test processes.

To solve this problem, I created a simple Windows Application for Windows 7 and 10 machines which reports the hardware used by this machine automatically using C#, XML, and XSD.

With this solution, nobody needs to send emails for finding a hardware anymore. Here is the regarding generic tool designed to collect and report hardware information of a PC:

<a href="https://gitlab.com/birolcapa/inventory-reporter-tool">https://gitlab.com/birolcapa/inventory-reporter-tool</a>