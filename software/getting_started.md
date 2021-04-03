# Software Getting Started

Software is divided into two main categories: software which runs on the phone and software which runs on a standard computer. Firmware isn't included in this since it's covered in electrical. Software which runs on the phone is packaged as Apps, and is mainly used to control the phone, perform autonomous functionality, and run in real time. Software on a standard computer is designed to run simulators, perform machine learning, and do various other offline tasks.

## App Getting Started

In order to get PhoneBot running the minimal that needs to be done is building an App and deploying it to your phone. We've developed apps with three different platforms, and have settled on [Flutter](https://flutter.dev/) as an App development environment since it enables cross platform development which is fast enough for real-time work. However, we still support our older Android and Kivy apps, and have instructions to build those as well.

- [Flutter Getting Started](app/flutter_app.md)
  - The current app being developed. It doesn't currently have all of the features of previous apps such as leg control, camera interfaces and face following.
- [Kivy Getting Started](app/kivy_app.md)
  - Contains the Face Following demo using OpenCV on an Android device.
- [Android Getting Started](app/android_app.md)
  - Contains the Leg Control demo, as well as reliable BLE connection and control.

## Desktop Getting Started

TODO
