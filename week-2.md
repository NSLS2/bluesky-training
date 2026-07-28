# Week 2

_Focus: The data side of the RunEngine_

During review of Week 1, we looked again at IPython, specifically at
_profiles_. The command `ipython --profile ...` corresponds to a directory
named `~/.ipython/profile_...` that contain startup scripts, a database of
command history, and so on.

We noted that `bsui` started as a bash alias, a shorthand to activate an
environment (`pixi run --manifest-path ...` or formerly `conda activate ...`)
and start `ipython` with `--profile collection`. 

Reminder: This drops the data; it goes nowhere.

```python
from bluesky import RunEngine
RE = RunEngine()
from ophyd.sim import det, motor
from bluesky.plans import count, scan
RE(count([det]))
```

This prints the documents' types (names) and contents.

```python
RE(count([det]), print)
```

We can write a custom callback to only print the document type (name).

```python
def cb(name, doc):
    print(f"type: {name}")
```

```python
RE(count([det]), cb)
RE(scan([det], motor, 1, 3, 3), cb)
```

There are callbacks that produce more polished output.

```python
from bluesky.callbacks import LiveTable
RE(scan([det], motor, 1, 3, 3), LiveTable(['motor', 'det']))
```

The "best-effort" callback uses heuristics to guess what data
to show.

```python
from bluesky.callbacks.best_effort import BestEffortCallback
bec = BestEffortCallback()
RE(scan([det], motor, 1, 3, 3), bec)
```

Integration teams can write custom plans and custom callbacks
fit to purpose.

```python
def xanes_scan(start, end, num=50):
    yield from scan([det], motor, start, end, num)
RE(xanes_scan(1, 3))
```

```python
from ophyd.sim import img
img.stage()
img.trigger()
img.read()
list(img.collect_asset_docs())
RE(count([img]), cb)
```
