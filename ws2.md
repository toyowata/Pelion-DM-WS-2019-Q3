# Workshop 2 : Network Socket

このワークショップでは、以下を行います

* Ethernet接続用のサンプルコードをインポート
* Network Socket APIを使用し、サンプルコードをWi-Fi接続に変更
* ビルドとターゲットボードへの書き込み
* シリアルターミナルでアプリケーションの実行を確認

## Echo Server サンプルアプリケーション

![](./pict/echoserver.png)

（参考）  
https://en.wikipedia.org/wiki/Echo_Protocol

## インポートとビルド手順

このワークショップのサンプルコードはGitHubのリポジトリではありません。以下のコマンドでインポートします。

```
$ mbed import https://os.mbed.com/teams/Mbed-IoT-workshop/code/mbed-os-workshop-sockets
$ cd mbed-os-workshop-sockets
$ mbed compile -m DISCO_L475VG_IOT01A -t gcc_arm
```

このサンプルコードは、Ethernet用なので、DISCO_L475VG_IOT01A でビルドするとリンクエラーが発生します。

```
Link: mbed-os-workshop-sockets
BUILD/DISCO_L475VG_IOT01A/GCC_ARM/main.o: In function `__static_initialization_and_destruction_0':
/Users/toywat01/test/work/mbed-os-workshop-sockets/./main.cpp:7: undefined reference to `EMAC::get_default_instance()'
collect2: error: ld returned 1 exit status
```
これは、コード中で使用されているEthernetInterfaceのシンボルが見つからないためです。

```cpp
// TODO: include corresponding network driver header file
#include "EthernetInterface.h"

// TODO: define network interface object
EthernetInterface net;
```

# Wi-FI接続への変更方法

## Wi-Fiドライバの検索

以下のサイトを表示します。  

https://os.mbed.com/platforms/ST-Discovery-L475E-IOT01A/

featuresセクションから、Wi-Fiに関する記載を調べます。

![](./pict/wifi.png)

GitHubのArmMbed配下で、`ISM43362`を検索します。以下のドライバが見つかります。

https://github.com/ARMmbed/wifi-ism43362

## ドライバのリポジトリ名とヘッダファイルの検索

* GitHubリポジトリ名 : wifi-ism43362
* ヘッダファイル名 : ISM43362Interface.h

以下のコマンドでWi-Fiドライバを追加します。

```
$ mbed add wifi-ism43362
```

## Wi-Fi接続用のコンフィグレーション

Wi-Fi用のサンプルコードの設定を参照にするのが簡単な方法です。

https://github.com/ARMmbed/mbed-os-example-wifi

![](./pict/wifi_config.png)

## コンフィグレーションのヒント

https://github.com/ARMmbed/mbed-os-example-wifi/blob/700c55a8af59adc649626a903328830b48aa0b4f/main.cpp#L96

https://github.com/ARMmbed/mbed-os-example-wifi/blob/700c55a8af59adc649626a903328830b48aa0b4f/mbed_app.json#L1-L17

