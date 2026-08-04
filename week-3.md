# Week 3

Run a "simple" Tiled server on a background thread.
Make a space for writing raw Bluesky data.

```
from tiled.client import simple
c = simple()
raw = c.create_container('raw', specs=['CatalogOfBlueskyRuns'])
```

Wire up a RunEngine to write to Tiled.

```python
from bluesky import RunEngine
from bluesky_tiled_plugins import TiledWriter

RE = RunEngine()
tw = TiledWriter(c['raw'])
RE.subscribe(tw)
```

Take data and access it.

```python
from bluesky.plans import count
from ophyd.sim import det
RE(count([det]))
raw
run = raw[-1]
run
run['primary']
run['primary']['det']
run['primary']['det'][:]
run['primary']['det'].read()
list(run)
for stream in run:
    print(stream)

import collections.abc
isinstance(run, collections.abc.Mapping)
run
run['primary']
run['primary']]
run['primary']['det']
run['primary']['det'].read()
run['primary'].read()
ds = run['primary'].read()
ds['det']
ds['time']
run
run.metadata
run['primary'].metadata
run
run['
run['primary']
from ophyd.sim import img
RE(count([img]))
c
raw
c[3]
raw[3]
raw[3]['primary']
raw[3]['primary']['img']
raw[3]['primary']['img'][:]
c = simple(readable_storage=['/tmp'])
c.create_container('raw', specs=['CatalogOfBlueskyRuns'])
tw = TiledWriter(c['raw'])
RE = RunEngine()
RE.subscribe(tw)
RE(count([img]))
raw = c['raw']
raw[1]['primary']
raw[1]['primary']['img']
raw[1]['primary']['img'][:]
#mon-tiled
raw[1]['primary']['img'].data_sources()
import numpy
raw[1].metadata
det
det # upload
img # register
RE(count([det, img]))
raw[1]['primary']
raw[1]['primary']['img'].data_sources()
raw[1]['primary']['img'][:3, :3]
raw[1]['primary']['img'][:, :3, :3]
c.write_array([1,2,3], metadata={'x.y': 3})
RE(count([motor, det], 7))
from ophyd.sim import motor
RE(count([motor, det], 7))
from bluesky.callbacks import LiveTable
RE(count([motor, det], 7), LiveTable(['motor', 'det']))
run = raw.values().last()
run
run['primary']
ds = run['primary'].read()
ds
from bluesky.plans import scans
from bluesky.plans import scan
RE(scan([det], motor, -3, 3, 7), LiveTable(['motor', 'det']))
run = raw.values().last()
run['primary']
ds = run['primary'].read()
ds
ds
c = from_uri('https://tiled.nsls2.bnl.gov', 'dask')
r = c['smi/migration'].values().last()
r
r['primary']
r['primary']['pil2M_image']
ac = r['primary']['pil2M_image']
ac
ds = r['primary'].read()
ds
```
