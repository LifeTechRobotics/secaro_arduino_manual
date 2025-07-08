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

# 利用環境

- Windows 11  
- Python 3.8  

---

# リモート操作前の準備

## ラジコン操作画面を起動する

1. サンプルプログラムの `controller` フォルダを開き、`controller.exe` をダブルクリックします。  
   ![0](../images/remote/0.png)

2. リモート操作画面が表示されます。  
   ![1](../images/remote/1.png)

## Atom Lite を起動する

**Arduino IDEの導入 > <a href="../enviroment/#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E5%8B%95%E3%81%8B%E3%81%97%E6%96%B9" target="_blank" rel="noopener noreferrer">サンプルプログラムの動かし方</a>** を参照し、Atom Lite を起動します。

- 利用するサンプルプロジェクトファイル：`remote/remote.ino`
- サンプルプログラムは、**<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードしてください。

---

# リモート操作してみる

## ロボットに接続する

1. リモート画面の **デバイスをスキャン** ボタンをクリックし、ロボットを検索します。  
   「スキャン中」が表示され、しばらくすると検出された Bluetooth デバイス一覧が表示されます。  
   ![2](../images/remote/2.png)

2. 一覧から名前が **Secaro(xx:xx:xx:xx:xx:xx)** のデバイスを選び、指示に従って手動でペアリングを行います。  
   **ペアリングが完了するまで OK ボタンは押さないでください。**  
   ![3](../images/remote/3.png)

3. Windows 設定を開き、**Bluetooth とデバイス** → **デバイスの追加** を選び、Bluetooth デバイスの追加を行います。  

   ![4](../images/remote/4.png)  

   ![5](../images/remote/5.png)  

   ![6](../images/remote/6.png)  
   
   ![7](../images/remote/7.png)  

   デバイスの状態が「**ペアリング済み**」になれば、接続完了です。  
   ![8](../images/remote/8.png)

4. リモート操作画面に戻り、**OK** ボタンをクリックします。  
   デバイス Secaro のステータスが「**接続済み**」になれば、操作可能な状態です。  
   ![9](../images/remote/9.png)

---

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

---

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

---

## ファームウェア構成

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

---

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

---

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
