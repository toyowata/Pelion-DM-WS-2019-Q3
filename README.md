# Pelion Device Management ハンズオンワールショップ

# Workshop 1: Mbed CLIのインストールとビルド

## インストール

以下のリンクを参照し、使用するホストマシン用のインストーラを使用してください。

https://os.mbed.com/docs/mbed-os/v5.13/tools/installation-and-setup.html

インストール後に、最新版のモジュールにアップデートします。

```
$ pip install -U mbed-cloud-sdk mbed-cli manifest-tool
```


## Windowsのみ：MAX_PATH制限とGCC_ARMでのコンパイルエラー

Windowsのデフォルトの設定では、ファイルパスが長すぎるのでコンパイルエラーが発生する場合があります。このコンパイルエラーを避けるため、以下のグループポリシーを “Enabled" に設定します。  
* MMC （Windowsキー+R で gpedit.msc を指定）> Local Computer Policy > Computer Configuration > Administrative Templates > System > Filesystem:  
* `Enable Win32 long paths` をダブルクリックし、Enabled に設定する  
* コマンドラインで、`gpupdate /force` を実行する（その後、ログオフする）  

（参考）
https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/  
https://docs.microsoft.com/en-gb/windows/desktop/FileIO/naming-a-file  
https://bugs.python.org/issue27731  

## サンプルコード Blinky をインポートする

* ターミナルやコマンドプロンプトを開く
* 作業用ディレクトリのルートに移動する

```
$ mbed import http://github.com/ARMmbed/mbed-os-example-blinky
$ cd mbed-os-example-blinky
```

* USBケーブルでボードとホストマシンを接続し、ボードを検出する
```
mbed detect
```
以下の情報が表示されます。
```
[mbed] Detected DISCO_L475VG_IOT01A, port /dev/tty.usbmodem14203, mounted /Volumes/DIS_L4IOT, interface version 0221:
[mbed] Supported toolchains for DISCO_L475VG_IOT01A
| Target              | mbed OS 2 | mbed OS 5 |    uARM   |    IAR    |    ARM    |  GCC_ARM  | ARMC5 |
|---------------------|-----------|-----------|-----------|-----------|-----------|-----------|-------|
| DISCO_L475VG_IOT01A | Supported | Supported | Supported | Supported | Supported | Supported |   -   |
Supported targets: 1
Supported toolchains: 4
```

## サンプルコードをビルドする

```
mbed compile -t <toolchain> -m <module>  
```
このワークショップではtoolchainに、GCC_ARMが指定可能です。moduleには、mbed detectコマンドで表示された Target名を使用します。

実行例:
```
$ mbed compile -t GCC_ARM -m DISCO_L475VG_IOT01A
```

ビルドされたバイナリファイルをドラッグアンドドロップする、または以下のようにコマンドを実行する。

```
cp BUILD/<target>/<toolchain>/mbed-os-example-blinky.bin <mount point>
```
実行例: 

```
$ cp BUILD/DISCO_L475VG_IOT01A/GCC_ARM/mbed-os-example-blinky.bin /Volumes/DIS_L4IOT/ (MacOS) 
$ copy BUILD/DISCO_L475VG_IOT01A/GCC_ARM/mbed-os-example-blinky.bin D: (windows)
```

## シリアルポートをモニタする

様々なシリアルポートモニタのソフトウェアを使用可能です。例：CoolTerm、TeraTerm、Mbed CLI の “mbed sterm” コマンドが使用できます。

（ここに画像入れる）

## サンプルコードを変更してビルドする

アイディア
* シリアルポートに特定の文字列を表示する
* LEDの点滅パターンを変更する

編集するファイルは、サンプルコードのトップレベルディレクトリ (mbed-os-example-blinky) の main.cpp です。


# Workshop 2 : Network Socket


