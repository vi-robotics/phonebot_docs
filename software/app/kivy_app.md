# Kivy App Getting Started

When in doubt, navigate to the [Kivy App](https://github.com/vi-robotics/phonebot_kivy) and refer to the README.

The current instructions are as follows:

## Setup Permissions

First, enable docker permissions and verify that the user was added to the `docker` group:

```
sudo usermod -aG docker ${USER}
groups ${USER}
```

Note that the above setup may require a login/logout (restart?) in order for the group changes to take effect.

While you're at it, also ensure that the application can connect to the android device by adding yourself to the `plugdev` group:

```
sudo usermod -aG plugdev ${USER}
groups ${USER}
```

## Install Docker

Now that the permissions have been set up, build and start the docker image.

For instructions on installing docker in your OS, see the documentation [here](https://docs.docker.com/install/).

## Deploy to Android Phone

1. Navigate to the `KivyApp` root as referenced from repository root:

   ```
   cd "$(git rev-parse --show-toplevel)/App/KivyApp"
   ```

1. Build docker image:

   ```
   make docker
   ```

1. Enter docker shell:

   To avoid the specific commands, I created a `Makefile` entry.

   ```
   make docker-shell
   ```

1. Inside the docker image, install android sdk version if not already available:

   ```
   ./.buildozer/android/platform/android-sdk/tools/bin/sdkmanager "platforms;android-27" "build-tools;27.0.3" "extras;google;m2repository" "extras;android;m2repository"
   ```

1. Build the Android `.apk` file:

   ```bash
   make build
   ```

1. Deploy to the Android Phone and run the app (requires USB connectivity):

   ```
   make install
   ```
