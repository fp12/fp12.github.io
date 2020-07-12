---
layout: post
title: "A simple event system"
tags: yahtr python
date: 2016-11-17
---

As I didn't want my game logic to know about the UI system, I needed a simple way to let the UI know when game things happen.
<!--more-->

### Solution

I searched for existing Python event systems and [many](http://www.valuedlessons.com/2008/04/events-in-python.html) [sources](http://www.voidspace.org.uk/python/weblog/arch_d7_2007_02_03.shtml#e616) use the same principle: 

* a single class to register callbacks
* use the self *increment*/*decrement* operators to add/remove yourself to the event
* use the *call* operator to fire the event

So here is what you can find an other sites: a very clean *Event* class, which is very close to the one I use in [yahtr](https://github.com/fp12/yahtr).

``` python
class Event:
    def __init__(self, *args, **kwargs):
        self.handlers = set()

    def handle(self, handler):
        self.handlers.add(handler)
        return self

    def unhandle(self, handler):
        try:
            self.handlers.remove(handler)
        except:
            raise ValueError("Handler is not handling this event, so cannot unhandle it.")
        return self

    def fire(self, *args, **kargs):
        for handler in self.handlers:
            handler(*args, **kargs)

    __iadd__ = handle
    __isub__ = unhandle
    __call__ = fire
```

Note: I added **args* and ***kwargs* to the \_\_init\_\_ so I can expose the argument(s) at event declaration. No checks are ever done though.

Then you can use it like that:

```python
class MyEventSender:
    def __init__(self):
        # declare the event
        self.on_event = Event('arg1', 'arg2')
    
    def process(self, x, y):
        # fire the event
        self.on_event(x, y)

event_sender = MyEventSender()

class MyEventReceiver:
    def __init__(self):
        # register to the event with self callback
        event_sender.on_event += self.on_event_received
        
    def on_event_received(self, arg1, arg2)
        # receive the event here
        pass
```

Fabien.