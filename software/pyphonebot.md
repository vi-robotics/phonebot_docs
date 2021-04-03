# Pyphonebot Getting Started

Currently, most of the Phonebot developments have been done using the following setup of configurations:

- OS: `Ubuntu 18.04`
- Python: `Python 3.6`

There are likely other compatible versions, but the aforementioned combination is likely to work the best.

**NOTE** Currently WSL(Windows Subsystem for Linux) is not supported due to lack of GUI application development, which causes issues when running some of the demo apps. The core capabilities _may_ work to some extent, but do not expect the OS to be supported anytime soon.

## Pyphonebot setup

### Optional: Virtualenv

To control package dependencies, it could be a good idea to isolate your python packages inside a virtualenv.
There are multiple ways to achieve this, the simplest option is perhaps:

```
python3 -m venv env # will create env/ directory in $PWD
source env/bin/activate # enter virtualenv
```

### Install Dependencies

```
sudo apt-get install libgeos-dev # for shapely
pip3 install -r requirements.txt
# pip3 install -r requirements.txt --user # if not in a virtualenv, --user is recommended
```

For `OSX`, run `brew install geos` instead.

### Optional: Install as package

The `pyphonebot` package may be installed globally, so as to allow for easier development, as follows:

```
python3 setup.py install #--user
```

Note that the `--user` flag, here and elsewhere, limits the scope of the package locally to the current user.

Then uninstalled, as follows:

```
pip3 uninstall phonebot
```

### Run the demo application

The following script is intended to be relatively robust. May be run as a file or directly in the command line.

```
#!/usr/bin/env bash

# Get to the root directory regardless of where you are
pushd "$(git rev-parse --show-toplevel)/pyphonebot"

# Run the script
python3 -m phonebot.app.demo_pybullet

# Go back to where you were
popd
```

Afterwards, you should see the simulation and the visualizer running.
Following the default application control in `pybullet`, the available camera controls are as follows:

- `wheel{up,down}`: zoom view
- `ctrl+wheel`: pan view
- `ctrl+mousemove`: rotate view

TODO(yycho0108): Add figures
