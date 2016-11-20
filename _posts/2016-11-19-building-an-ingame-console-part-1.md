---
layout: post
title: "Building an ingame console (part 1)"
tags: yahtr python kivy
date: 2016-11-17
---

## Context

Ingame consoles have several usages: game logging, debug, commands...

In yahtr, I was losing some time constantly switching from the game to the System Console so I decided to do something for the long run and build an ingame console! With it I will be able to see my logs directly ingame, and afterwards, execute commands in it.

This post will be separated in 2 parts: how to feed the console and how to display it.
<!--more-->

Here is an example of the console in a Quake game.

![Quake Console](/images/quake-console.png)

## How to feed the console

I chose to use the [Python logging system](https://docs.python.org/3.6/library/logging.html), because it's widely used, very practical and I also found out that it's very open! If you don't know this module, I highly suggest that you check the doc and use it in your projects! You won't regret it!

From here, two options can be explored:
1. Create a specific logging handler that will take care of the log records directly
2. Create an agnostic logging handler that will fire an event when a log record needs to be handled

Obviously I chose #2 since I don't want to be dependant on my UI system. That said, you can find an example of #1 on [Stack Overflow](http://stackoverflow.com/questions/34539563/how-to-redirect-pythons-logging-output-to-kivy-label#34581893).

## Loggers architecture

Before diving into the details, remember that Python loggers can be hierachical:

```python
import logging

root_logger = logging.getLogger()
child_logger1 = logging.getLogger('child')
child_logger2 = root_logger.getChild('child')

assert child_logger1 is child_logger2
```

The advantage of hierachical loggers is that children share the same properties as their parents (if not overriden of course).
So let's prepare some loggers:

```python
# --- log.py
import logging

# parent logger for application related logs
# targeted to the system console
app_logger = logging.getLogger('app')

# parent logger for game related logs
# targeted to the ingame console AND will inherit from the app_logger handling (system console)
game_logger = app_logger.getChild('game')


# --- loading.py
from log import app_logger

loading_logger = app_logger.getChild('loading')


# --- battle.py
from log import game_logger

battle_logger = game_logger.getChild('battle')
```

## Loggers handling

Now this is a great way to organize loggers but it lacks the main part for our topic: the handler(s)...

The app_logger will be handled by the basic StreamHandler while the game_logger will be handled by a custom handler which job is to fire events with the handled record.

```python
import logging
import logging.handlers
from utils.event import Event

class GameConsoleHandler(logging.Handler):
    def __init__(self, level=logging.NOTSET):
        logging.Handler.__init__(self, level=level)
        self.on_message = Event('message')

    def emit(self, record):
        msg = self.format(record)
        # Broadcast the formatted message
        self.on_message(msg)

game_console_handler = GameConsoleHandler()

app_logger = logging.getLogger('app')
app_logger.addHandler(logging.StreamHandler())

game_logger = app_logger.getChild('game')
game_logger.addHandler(game_console_handler)
```

As you may have noticed, I use the *Event* class from my [previous post]({{ site.baseurl }}{% post_url 2016-11-17-a-simple-event-system %})
. Feel free to use another event-based system instead.


## The Kivy case

Kivy uses the official Python logging module as well, and you may need to tweak the previous code depending on your needs. Indeed, if you don't want to use the kivy Logger, you'll need to get a child from the root logger because, for some reason, the [Kivy logger is set as the root](https://github.com/kivy/kivy/blob/master/kivy/logger.py).

Here is the final code for this part:

```python
import logging
import logging.handlers
from utils.event import Event

class GameConsoleHandler(logging.Handler):
    def __init__(self, level=logging.NOTSET):
        logging.Handler.__init__(self, level=level)
        self.on_message = Event('message')

    def emit(self, record):
        msg = self.format(record)
        # Broadcast the formatted message
        self.on_message(msg)

game_console_handler = GameConsoleHandler()

app_logger = logging.getChild().getLogger('app')  # taking the root's child
app_logger.addHandler(logging.StreamHandler())
app_logger.propagate = False  # don't propagate to kivy's logging system

game_logger = app_logger.getChild('game')
game_logger.addHandler(game_console_handler)
```

This concludes this first part!