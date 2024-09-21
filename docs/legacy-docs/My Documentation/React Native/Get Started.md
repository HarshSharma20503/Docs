# Create react-native app
Use the following command to create an react-native app
```console
npx react-native init app_name --verbose
```

after that go inside the folder and perform `npm install` and `npm start`

now run the simulator by `npx react-native run-android`

# Check devices
Android Debug Bridge ( adb ) is a versatile command-line tool that lets you communicate with a device
use the following command to use adb
`adb --version` to check version
`adb --devices` to check which devices are connected
`lsusb` also to check devices
`echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="22b8", MODE="0666", GROUP="plugdev"' | sudo tee /etc/udev/rules.d/51-android-usb.rules` to change the rules of running simulator first and making it run on the connected device first priority.

# How to Use the USB cable and get output on your mobile device
Follow the given steps
https://reactnative.dev/docs/running-on-device

