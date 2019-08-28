# Workshop 4 : Firmware update

## Why is the hands-on different?
* No multi device campaign. Everyone update their own single device.
* *Not preceded by Factory Provisioning with FCU, use mbed dm init to generate
VendorID, ClassID, firmware integrity certificate.
* No Mbed Cloud Portal GUI, use API with Mbed Cloud API key to directly talk to Mbed
Cloud API Gateway using mbed dm update command.
* Hands-on workshop only. Do not generate a private key with no hardware protection on a computer and directly connect it to the internet - especially if that private key can sign your manifests and all your devices in the field with the certificate with the corresponding private key will trust whatever it signs.

## 新しいファームウェアをビルドする

* Ensure that you have completed the hands-on session "Mbed CLI Device Management“
* This relies on mbed dm init having been run before the initial firmware build
* Edit the main.cpp file so that it prints something new to the serial terminal, so you can tell the update has succeeded.
* Recompile the application

## Firmware update process
* Write the new firmware update and compile the binary
* Using the manifest-tool with the cloud SDK behind the “dm” command:
* Create a manifest
* Publish the manifest to Pelion Device Management
* Upload the new firmware to Pelion Device Management by looking for the *_update.bin file in the
build directory for the specified target device.
* Start the campaign for the selected device(s)

## Firmware update commands

(Before running update, connect with Putty to serial console to see the process in action)

* Ensure that your API key is set (if using a new command terminal since it was previously set)
```shell
# mbed config -G CLOUD_SDK_API_KEY <api key>
```
* Run the mbed dm update command from the “example” directory (for DISCO it is
c:\pelion-example-disco-iot01)
```
mbed dm update device -D <device_id> -t <toolchain> -m <target>
```
• For example:
```
mbed dm update device -D 016c04e037b100000000000100100372 -t GCC_ARM -m DISCO_L475VG_IOT01A
```
The Device ID can be found in the Portal, or the serial output from the device. Device ID and Endpoint Name by default is identical with Developer Flow (Factory likely different)

## Manifest tool output

## Device serial console at start of update process

## Device serial console at end of update process

## Troubleshooting

* Check the device ID in the mbed dm update command is correct.
* Check the device is still connected (you can see resources updating in portal)
* If you have had to cancel previous attempts (e.g. with ctrl-C) use the portal to delete any manifests, firmware and campaigns that may have been left around

## Optional – update using the Portal

* Do try to do this if you have time – it shows you the steps that the mbed dm command is doing for you.
* The process is described in the documentation from here: https://www.pelion.com/docs/device-management/current/updating-firmware/preparing-manifests.html
* steps:
    * [first edit main.cpp to print something new, and rebuild the firmware]
    * using the Portal, upload the firmware
    * use manifest-tool on the command line to create the manifest
    * upload