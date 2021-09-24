---
layout: post
title:  "How to read .evtx file using python?"
date:   2021-09-24 21:29:00 +0200
categories: software
---

I needed to read Windows Eventlog files using python.  
I was lucky because some people already had a similar problem:  
<https://stackoverflow.com/questions/43005100/how-to-read-evtx-file-using-python>

I changed the accepted answer a bit as following:  
```python
import xml.etree.ElementTree as Et
import win32evtlog
from collections import namedtuple
```

```python
class EventLogParser:
    def __init__(self, exported_log_file):
        self.exported_log_file = exported_log_file

    def get_all_events(self):
        windows_events = []
        query_handle = win32evtlog.EvtQuery(str(self.exported_log_file),
            win32evtlog.EvtQueryFilePath | win32evtlog.EvtQueryReverseDirection)
        while True:
            raw_event_collection = win32evtlog.EvtNext(query_handle, 1)
            if len(raw_event_collection) == 0:
                break
            for raw_event in raw_event_collection:
                windows_events.append(self.parse_raw_event(raw_event))
        return windows_events

    def parse_raw_event(self, raw_event):
        xml_content = win32evtlog.EvtRender(raw_event, win32evtlog.EvtRenderEventXml)
        root = Et.fromstring(xml_content)
        ns =  "{" + root.tag.split('}')[0].strip('{') + "}"
        system = root.find(f'{ns}System')
        event_id = system.find(f'{ns}EventID').text
        level = system.find(f'{ns}Level').text
        time_created = system.find(f'{ns}TimeCreated').get('SystemTime')
        computer = system.find(f'{ns}Computer').text
        WindowsEvent = namedtuple('WindowsEvent',
            'event_id, level, time_created, computer')
        return WindowsEvent(event_id, level, time_created, computer)

```