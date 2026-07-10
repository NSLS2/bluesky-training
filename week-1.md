# Week 1

## IPython

- Bluesky first targeted IPython as its user interface, with the SPEC user
  experience in mind.
- IPython is an enhanced Python terminal, purpose-built for scientists doing
  interactive science in Python.

## Ophyd

- Ophyd is Bluesky's hardware abstraction layer.
- It provides a higher-level abstraction than EPICS, and it can be backed
  by control systems other than EPICS.
- We implement simulated objects with no control system at all, purely in
  Python.
- Status objects are like Futures.

```python
from ophyd.sim import det, motor
```

```python
det.describe()
det.read()

motor.describe()
motor.read()

status = motor.set(3)
status = det.trigger()

motor.summary()
```

## Run Engine

- Bluesky plans are generators that yield `Msg` objects.
- Plans can be introspected and should not directly
  engage with hardware. Rather they should describe _what_
  to do, and let the RunEngine do it (mediated by error
  handling, etc).
- The RunEngine consumes an iterable of `Msg` objects.

```python
from bluesky import RunEngine
from ophyd.sim import motor, det

RE = RunEngine()

RE([Msg('set', motor, 3)])

from bluesky.plans import count

list(count([det]))
RE(count([det]))

from bluesky.plan_stubs import trigger_and_read

list(trigger_and_read([det]))

import random

def plan():
    for _ in range(3):
        if random.random() > 0.5:
            break
        yield from count([det])

RE(plan())
```
