---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: ラジコン操作
description: ロボットを遠隔操作してみましょう。

# Author box
# author:
#     title: About Author
#     title_url: '#'
#     external_url: true
#     description: Author description

# Micro navigation
micro_nav: true

# Page navigation
page_nav:
    prev:
        content: 4. ロボット動作
        url: /robot
    next:
        content: 6. 自律走行
        url: /auto
---

# サンプルプログラム

## サンプルプログラムのダウンロード
- リモート操作用プログラム（Windows）  
      `controller` 

- ファームウェア  
     `remote/remote.ino`  

- ダウンロード先  
     **<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードください。


## サンプルプログラムができること

Windows アプリケーションのラジコン操作画面を使用し、Bluetooth 通信を通じてロボットに接続します。
これにより、ロボットの走行方向および速度をリモートで制御することが可能です。


## 走行動画

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1101537021?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="5,ラジコン操作デモ"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

# リモート操作前の準備

## ラジコン操作画面を起動する

### 実行環境

- Windows 11  

### 実行手順

1. サンプルプログラムの `controller\exe` フォルダを開き、`controller.exe` をダブルクリックします。  
   ![0](../images/remote/0.png)

2. リモート操作画面が表示されます。  
   ![1](../images/remote/1.png)

## ロボットを動かす

必要に応じて **Bluetoothデバイス名を変更** した後、**Arduino IDEの導入 > <a href="../enviroment/#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E5%8B%95%E3%81%8B%E3%81%97%E6%96%B9" target="_blank" rel="noopener noreferrer">サンプルプログラムの動かし方</a>** を参照し、ロボットを動かします。


### Bluetoothデバイス名の変更方法

出荷時のロボットは、Bluetooth デバイス名がすべて「Secaro」に設定されています。単体で使用する場合は問題ありませんが、複数台のロボットを同時に使用する環境では、接続時に識別できるよう、それぞれ異なる名前に変更する必要があります。

#### デバイス名の変更手順

1. Arduino IDE で **ファイル > 開く** をクリックし、プロジェクトファイル（`remote.ino`）を選択します。 
   ![10](../images/remote/10.png)

   ![11](../images/remote/11.png)

2. キーボードの `Ctrl + F` を押し、検索バーに `Secaro` を入力します。  
3. 以下の定義部分が見つかります。

   ```cpp
   #define DEVICE_NAME "Secaro"
   ```
4. 他のロボットと名前が重複しないよう、"Secaro" の部分を任意の名前に変更してください。例えば：

   ```cpp
   #define DEVICE_NAME "Secaro_01"
   ```

これにより、Bluetooth接続時にロボットを個別に識別できるようになります。  


# リモート操作してみる

## ロボットに接続する

1. リモート画面の **デバイスをスキャン** ボタンをクリックし、ロボットを検索します。  
   「スキャン中」が表示され、しばらくすると検出された Bluetooth デバイス一覧が表示されます。  
   ![2](../images/remote/2.png)

2. 一覧から名前が **Secaro(xx:xx:xx:xx:xx:xx)** のデバイスを選び、「Bluetoothデバイスをペアリングしてください」とのメッセージが表示されます。
   **OKボタンを押さない**で、次のステップ3の手順に従って手動でペアリングを行ってください。  
   ![3](../images/remote/3.png)

3. Windowsの **設定** を開き、**Bluetooth とデバイス** → **デバイスの追加** を選び、Bluetooth デバイスの追加を行います。  

   ![4](../images/remote/4.png)  

   ![5](../images/remote/5.png)  

   ![6](../images/remote/6.png)  
   
   ![7](../images/remote/7.png)  

   デバイスの状態が「**ペアリング済み**」になれば、接続完了です。  
   ![8](../images/remote/8.png)

4. リモート操作画面に戻り、**OK** ボタンをクリックします。  
   デバイス Secaro のステータスが「**接続済み**」になれば、操作可能な状態です。  
   ![9](../images/remote/9.png)



## ロボットを動かす

### 速度指定

左右それぞれの車輪について、スライダーで速度（1〜9段階）を指定できます。  
![10](../images/remote/slider.png)

### 進行方向指定

以下のボタンで移動方向を指定します：

- 前進　　![up](../images/remote/up.png)  
- 後退　　![down](../images/remote/down.png)  
- 左旋回　![left](../images/remote/left.png)  
- 右旋回　![right](../images/remote/right.png)

### 停止

中央の停止ボタンをクリックすると、ロボットが停止します。  
![stop](../images/remote/stop.png)



# プログラミング

## ラジコン操作画面

- サンプルプログラムは `controller/src` に格納されています。
- サンプルプログラムは、**<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードしてください。

```yaml
src/
├── controller.py        # ボタン動作などを記述するPythonファイル
├── ui.kv                # GUIを描画するためのKivyファイル
└── ipaexg.ttf           # 日本語フォントファイル
```


## ファームウェア構成

### 依存関係

このプログラムは、Bluetooth 経由でコマンドを受信し、M5Atom 経由で 2 つの連続回転サーボモータを制御するものです。実行には以下の環境が必要です。


#### 使用ハードウェア

- **M5Atom Lite（ESP32ベース）**  
  M5Stack 社製の小型マイコン。Bluetooth 通信機能と PWM 制御機能を持つ ESP32 を搭載しています。

- **360度連続回転サーボモータ × 2**  
  PWM 信号のデューティ比によって回転速度と方向を制御できるタイプのサーボモータです。


#### 必要ライブラリ

- **M5Atom ライブラリ**  
  M5.begin() など、M5Atom シリーズ向けの機能を提供します。  
  ※**Arduino IDE** でインストールしておく必要があります。(**Arduino IDEの導入 > <a href="../enviroment/#%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB" target="_blank" rel="noopener noreferrer">ライブラリのインストール</a>** )

- **esp32-hal-ledc.h**  
  ESP32 の PWM（LEDC）制御に必要なヘッダファイルです。  
  ※Arduino Core for ESP32 に標準で含まれており、別途インストールは不要です。

- **BluetoothSerial**  
  ESP32 の Bluetooth Classic 通信機能を提供します。  
  ※Arduino Core for ESP32 に標準で含まれており、別途インストールは不要です。

#### ピン設定

| 機能              | GPIOピン |
|-------------------|----------|
| サーボモータ1     | 19       |
| サーボモータ2     | 22       |


### setup()

各種初期化処理を行います。  
Atom Lite とラジコン画面は Bluetooth で接続されるため、Bluetooth デバイスの初期化が含まれます。

```cpp
#define DEVICE_NAME "Secaro"   // Bluetoothデバイス名

BluetoothSerial SerialBT;

void setup() {
    // Bluetooth待ち受け開始
    SerialBT.begin(DEVICE_NAME);
    delay(200);
    SerialBT.setTimeout(2000);
}
```


### loop()

ラジコン操作画面からの指令を受け取り、車輪を制御します。

#### 指令の種類

操作画面から受け取るコマンドは以下の通りです：

| コマンド | 内容             |
|----------|------------------|
| `F`      | 前進             |
| `B`      | 後退             |
| `L`      | 左旋回           |
| `R`      | 右旋回           |
| `S`      | 停止             |
| `P`      | 通信確認         |
| `lx`     | 左輪速度指定（x=1〜9） |
| `rx`     | 右輪速度指定（x=1〜9） |



#### 速度制御

Servo Kit 360° の停止となるデューティ比は約 `307`。  
ただし、実際には `290〜313` の範囲であれば停止状態とみなされます。より正確に制御するために閾値を設けています。

```cpp
const int centerDuty = (int)(4096 * 1.5 / 20.0); // 約307
const int centerDutyHigh = 313;  // 停止範囲の上限
const int centerDutyLow  = 290;  // 停止範囲の下限

void loop() {
    // 例：前進
    ledcWrite(PIN_1, centerDutyHigh + step * spdL);
    ledcWrite(PIN_2, centerDutyLow  - step * spdR);
}
```
