# Developing Android apps on Gitpod
This project is intended to demonstrate how to easily program Android apps on Gitpod. It is based on [this guide](https://sometechy.website/dev/adb-over-any-network-without-port-forwarding-even-over-mobile-3g-4g-lte.html) and [this flutter template](https://github.com/gitpod-io/flutter-example). This image already enables you to program for Flutter Web - in this guide however I want to focus on programming on a real device.

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/JamesCullum/flutter-adb-template)

## Requirements

 - Gitpod (or other cloud IDE)
 - Android phone (Android 11+)
 - ngrok (or other tunneling software)
 - Wi-fi connection between the current device (laptop) and the phone

## Step 1 - Enable web debugging
References: [Gadgethacks - Picture Source](https://android.gadgethacks.com/how-to/set-up-wireless-debugging-android-11-send-adb-commands-without-usb-cable-0302898/) / [Gitconnected](https://levelup.gitconnected.com/wireless-debugging-in-android-11-7169d2596a81)

On your Android 11+ phone, enable developer settings by tapping the build number multiple times. After the developer settings, are unlocked, open them up and enable "Wireless debugging". Once this is enabled, click on the text next to the toggle to get to the details of this setting. 

![Enable wireless debugging](https://img.gadgethacks.com/img/52/64/63724404485511/0/set-up-wireless-debugging-android-11-send-adb-commands-without-usb-cable.w1456.jpg | height=500) ![Allow wireless debugging](https://img.gadgethacks.com/img/15/73/63724404499027/0/set-up-wireless-debugging-android-11-send-adb-commands-without-usb-cable.w1456.jpg | height=500)

Write down the displayed IP and port - this is the connection for the main ADB connection.

![Main ADB connection interface](https://img.gadgethacks.com/img/40/29/63724411271707/0/set-up-wireless-debugging-android-11-send-adb-commands-without-usb-cable.w1456.jpg | height=500)

Click on "Pair device with pairing code" below - you will see a pairing code, the same IP address and a different port. Write these down as well and keep this window open. This is the connection only for pairing.

![Pairing ADB connection interface](https://img.gadgethacks.com/img/94/18/63724411291019/0/set-up-wireless-debugging-android-11-send-adb-commands-without-usb-cable.w1456.jpg | height=500)

## Step 2 - Tunnel ADB via ngrok

Download [ngrok](https://ngrok.com/). Open a terminal on your computer, that must be in the same wifi network like the phone. You can verify this by pinging the phone IP address like `ping 192.168.178.33` - if the ping succeeds, your network settings are correct.

Run ngrok to tunnel the pairing connection first, for example 
`ngrok tcp 192.168.178.33:42739`. ngrok will provide you with an external domain and port, like `0.tcp.ngrok.io:13840`.

![ngrok sample screen](https://forums.triplea-game.org/assets/uploads/files/1589396698766-e9c19651-391d-4f53-8119-35340257a5f2-grafik.png)

Open your Android code in your cloud IDE with adb installed - like [this example](https://gitpod.io/#https://github.com/JamesCullum/flutter-adb-template). Use the command line terminal in the IDE to pair the IDE with the device like `adb pair 0.tcp.ngrok.io:13840`. Enter the pairing code and wait until the connection is confirmed.

Terminate the ngrok tunnel for the pairing connection (CTRL + C in the local terminal). Now create a new tunnel for the main ADB connection, like `ngrok tcp 192.168.178.33:37829` which will provide you with a new domain and port like `6.tcp.ngrok.io:16514`.

Now connect to the main ADB connection via `adb connect 6.tcp.ngrok.io:16514` - this requires the previous pairing step to succeed. The connection should establish successfully.

## Step 3 - Run the app
Once the connection is established, you can use your phone in combination with the IDE to work on apps as usual. 

In the example mentioned above, you can list all connected devices via `flutter devices`. Build and push a development build to the device via `flutter run`. This will compile the code, pushes it to the phone via the internet and launches it on the phone.

![Flutter Programming View](https://camo.githubusercontent.com/6f5049176dc412060d5887e80874388f13fa9d4a43b0d340be9b4ed3ae6f0757/68747470733a2f2f64617274636f64652e6f72672f696d616765732f6d61726b6574706c6163652f666c75747465725f686f745f72656c6f61642e676966)
