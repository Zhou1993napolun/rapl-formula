# RAPL-formula (written by Tan Ying)

# Activate conda env
```
conda activate rapl
```

# Install (if not)
Install rapl
```
pip install rapl-formula
```
Install mongodb
```
sudo apt install mongodb-server-core
```
Install pymongo
```
pip install pymongo
```
# Start hwpc-sensor
Start mongodb server
```
mongod --dbpath /var/lib/mongo --logpath /var/log/mongodb/mongod.log --fork 
```
Write the configuration file for hwpc-sensor
```
vim /ty/config_file.json 
```
Start the hwpc-sensor
```
cd ty
```
```
sudo docker run --rm --net=host --privileged --pid=host -v /sys:/sys -v /var/lib/docker/containers:/var/lib/docker/containers:ro -v /tmp/powerapi-sensor-reporting:/reporting -v $(pwd):/srv -v $(pwd)/config_file.json:/config_file.json powerapi/hwpc-sensor --config-file /config_file.json
```

# Start RAPL-formula
Write the configuration file for RAPL-formula
```
vim /ty/config_file_rapl.json
```
Start RAPL-formula
```
python -m rapl_formula --config-file config_file_rapl.json
```





















# RAPL-formula (original)

A [powerAPI](https://github.com/powerapi-ng/powerapi) formula using RAPL
counters to provides power consumption information of each socket of the
monitored machine.

Use RAPL data collected with the
[hwpc-sensor](https://github.com/powerapi-ng/hwpc-sensor) and convert it into
power consumption measures (in Watt). The power consumption measures are store
in a MongoDB database.

# Quick start

We detail here how to quickly start rapl-formula and connect it to a hwpc-sensor
using a mongoDB instance.

For more detail see our documentation [here](http://powerapi.org)

## Get input data

You have to launch the [hwpc-sensor](https://github.com/powerapi-ng/hwpc-sensor) to
monitor sockets. The sensor must store its data in a mongoDB database. This
database must be accessible by the rapl_formula.

## Configuration

You can pass the configuration through a file or the CLI.
In both case the parameters to precises are the following :

- `verbose` (bool)
- `stream` (bool): If working with a sensor in real-time
- `input`
- `output`
- `enable-cpu-formula` (bool): Enable CPU formula', default=True
- `enable-dram-formula` (bool): Enable DRAM formula, default=True
- `'cpu-rapl-ref-event` (str): RAPL event used as reference for the CPU power models, default='RAPL_ENERGY_PKG'
- `'dram-rapl-ref-event` (str): RAPL event used as reference for the DRAM power models, default='RAPL_ENERGY_DRAM'
- `'sensor-report-sampling-interval` (int): (for stream mode) The frequency with which measurements are made
  (in milliseconds)
