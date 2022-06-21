# Workshop Setup

The workshop requires to build two Docker images: `tools` and `emulator`


## Docker: emulator

An Android 11 OS image with root privileges will be downloaded:

#### Build

```sh
make build-emu
```

Build in CyberTruck network:

```sh
make build-emu-local
```

#### Run

```sh
make shell-emu
```

Run in CyberTruck network:

```sh
make shell-emu-local
```

Run these commands inside the Docker emulator container:

```sh
sdkmanager --list
avdmanager create avd -n first_avd --abi google_apis/x86_64 -k "system-images;android-30;google_apis;x86_64"
emulator -avd first_avd -no-window -no-audio &
adb devices
```

## Docker: tools

Once you have running the emulator on Docker, then run this image and verify you can connect to the Android emulator via ADB.

#### Build

```sh
make build
```

Build in CyberTruck network:

```sh
make build-local
```

#### Run

```sh
make shell
```

Run in CyberTruck network:

```sh
make shell-local
```

Successful setup will look like this output:
```sh
$ make shell
docker run --network host -v /tmp:/tmp -it --entrypoint bash cbtruck
root@xps:/opt/cbtruck# adb devices
List of devices attached
emulator-5554	device
```
