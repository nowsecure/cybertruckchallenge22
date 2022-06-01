# Workshop Setup

The workshop requires to build two Docker images: `tools` and `emulator`


## Docker: tools

#### Build

```sh
make
```

#### Run

```sh
make shell
```

## Docker: emulator

An Android 11 OS image with root privileges will be downloaded:

#### Build

```sh
docker pull androidsdk/android-30
```

#### Run

```sh
docker run -it --rm --network host --device /dev/kvm androidsdk/android-30:latest bash
sdkmanager --list
avdmanager create avd -n first_avd --abi google_apis/x86_64 -k "system-images;android-30;google_apis;x86_64"
emulator -avd first_avd -no-window -no-audio &
adb devices
```
