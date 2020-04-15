---
id: 54
title: 'UI Automation Tool Helper: Microsoft Spy++'
date: 2017-01-22T06:03:22+00:00
author: birol
layout: post
guid: http://www.birolcapa.com/blog/?p=54
permalink: /2017/01/22/ui-automation-tool-helper-microsoft-spy/
categories: software
---
Microsoft makes a definition of Microsoft Spy++ as following:

Spy++ is a Windows based utility that shows a graphical view of the system’s processes, threads, windows, and window messages [1].

I have used the tool for understanding Colasoft Packet Player UI's windows.

First of all, I opened Colasoft Packet Player.

![Colasoft Index](/images/CS_Index.png)


After then, I opened the Spy++.
When I looked for Colasoft Packet Player text in Spy++, I found following picture:

![Colasoft Spy Image 1](/images/CS_Spy1.png)


![Colasoft Spy Image 2](/images/CS_Spy2.png)


This picture tells me that I can find Colasoft Packet Player’s Window handle by using the “Colasoft Packet Player” string.
When I get the Window handle, I can find the detailed information by using this Handle.

By using this handle,  
– I can get the “Combobox” and set its value.  
– I can get the “Textbox” and set its value.  
– I can get the “TrackBar” and set its value.  
– I can get the “Status Text Box” and get its value.  
By using this handle and “Play” string, I can find the Play button.  
By using this handle and “Close” string, I can find the Close button.  

How can I get these handles and send messages to these handles?

To be continued …

[1] https://msdn.microsoft.com/en-us/library/aa264396(v=vs.60).aspx