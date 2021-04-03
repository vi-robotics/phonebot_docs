# Troubleshooting

#### sudo: apt-get: command not found

In this case, you are likely not running some variant of `Ubuntu`. There is limited support for the OS outside of `Ubuntu 16.04 / 18.04`, but try substituting the `apt` or `apt-get` commands with `brew` equivalents (for `OSX`), for instance.

#### SyntaxError: invalid syntax

If you are running the scripts with `python`, verify that you are actually running `python3`, not `python2`:

```
python --version
```

Depending on the context, this may also mean that you would need to run `pip3` instead of `pip` to ensure that you are installing the packages for `python3`.

#### ModuleNotFoundError: No module named 'phonebot.core.common.math'

If you're getting an error message akin to the following:

```
pybullet build time: Jan 12 2020 16:28:02
Traceback (most recent call last):
  File "app/demo_pybullet.py", line 12, in <module>
    from phonebot.core.common.math.transform import Transform, Rotation, Position
ModuleNotFoundError: No module named 'phonebot.core.common.math'
```

Then you are likely not running the script from the correct directory. In our case, the directory from which all `pyphonebot` scripts are invoked is `Phonebot/pyphonebot/`, or `$(git rev-parse --show-toplevel)/pyphonebot` if you want to just get there directly regardless of your current location (as long as you are inside the git repository)

The recommended way to run the scripts is as follows:

```
python3 -m phonebot.app.demo_rotation_controller
```

#### Arduino IDE gives `Wrong microcontroller found. Did you select the right board from the Tools > Board menu?` error or some related issue of finding the incorrect device signature for the board when using Arduino in Linux.

This could be caused by Linux interpreting the chip as a modem. To fix, uninstalling `modemmanager` via the command `sudo apt remove modemmanager`.

#### Build Error in Docker Image

Permission Error, as such:

```
PermissionError: [Errno 13] Permission denied: '/home/user/.buildozer/cache'
```

Solution:

```
sudo chown -R $USER ~/.buildozer
```

#### Sdk Download Timeout

Symptom:

```
Could not get resource 'https://dl.google.com/dl/android/maven2/com/android/tools/sdklib/26.1.4/sdklib-26.1.4.jar'
```

Solution:
Run `make build` again.

#### Permissions error in `make install`

Symptom:

```
adb: error: failed to get feature set: insufficient permissions for device: user user is not in the plugdev group
```

Solution:

1. Ensure that your permissions have been correctly setup (`plugdev`).
1. Install any previously installed version of `PhonebotApp`.
1. Ensure that `USB Preferences` in `Android` is set to `File transfer / Android Auto`.
