---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: ロボット動作
description: ロボットを前後左右に動かしてみましょう。

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
        content: 3. サーボモータ動作
        url: /servo
    next:
        content: 5. ラジコン操作
        url: /remote
---

# サンプルプログラム

## サンプルプログラムのダウンロード

- 利用するサンプルプロジェクトファイル：`robot/robot.ino`  
- サンプルプログラムは、**<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードしてください。

## サンプルプログラムができること

ロボットは、次の順序で動作します。

1. 前進  
2. 後退  
3. 左旋回  
4. 右旋回  
5. 停止  

## 走行動画

<div style="padding:75% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1101542575?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="VID_20250715_134800621"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

# ロボットを動かしてみる

**Arduino IDEの導入 > <a href="../enviroment/#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E5%8B%95%E3%81%8B%E3%81%97%E6%96%B9" target="_blank" rel="noopener noreferrer">サンプルプログラムの動かし方</a>** を参照し、ロボットを動かします。

<div class="callout callout--danger">
    <p><strong>注意：ロボットが動きます！</strong></p>
    <p>書き込み直後にロボットが動くため、ケーブルが絡まないよう、書き込む前にロボットの車輪が浮くように手で持ち上げてください。</p>
</div>

# プログラミング

## 依存関係

このスケッチを正常に動作させるためには、以下のハードウェアおよびライブラリが必要です。

### 使用ハードウェア

- **M5Atom Lite（ESP32ベース）**  
  M5Stack 社製の小型マイコン。Bluetooth 通信機能と PWM 制御機能を持つ ESP32 を搭載しています。

- **360度連続回転サーボモータ × 2**  
  PWM 信号のデューティ比によって回転速度と方向を制御できるタイプのサーボモータです。


### 必要ライブラリ

- **M5Atom ライブラリ**  
  M5.begin() など、M5Atom シリーズ向けの機能を提供します。  
  ※**Arduino IDE** でインストールしておく必要があります。(**Arduino IDEの導入 > <a href="../enviroment/#%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB" target="_blank" rel="noopener noreferrer">ライブラリのインストール</a>** )
  
- **esp32-hal-ledc.h**  
  ESP32 の PWM（LEDC）制御に必要なヘッダファイルです。  
  ※Arduino Core for ESP32 に標準で含まれており、別途インストールは不要です。


### ピン設定

| 機能              | GPIOピン |
|-------------------|----------|
| サーボモータ1     | 19       |
| サーボモータ2     | 22       |


## setup()

各種初期化を行います。  
2つの車輪を動かすため、前章よりも使用する GPIO ピンが一つ増えます。ここでは GPIO 19 と 22 を利用します。

```cpp
const int PIN_1 = 19;          // 車輪サーボ1
const int PIN_2 = 22;          // 車輪サーボ2

void setup() {
    // LEDC PIN設定
    ledcAttach(PIN_1, FREQ, RESOLUTION);
    ledcAttach(PIN_2, FREQ, RESOLUTION);

    // 初期停止
    ledcWrite(PIN_1, centerDuty);
    ledcWrite(PIN_2, centerDuty);
    delay(1000);
}
```

---

## loop()

速度制御と方向制御を行います。  
以下のように各動作に応じた PWM 出力をサーボに送ります。

- **前進**
  - サーボ1：正転
  - サーボ2：逆転

- **後退**
  - サーボ1：逆転
  - サーボ2：正転

- **左旋回**
  - サーボ1：逆転
  - サーボ2：逆転

- **右旋回**
  - サーボ1：正転
  - サーボ2：正転

```cpp
void loop() {
  if(run == true) {
    // 前進
    ledcWrite(PIN_1, centerDuty + step);
    ledcWrite(PIN_2, centerDuty - step);
    delay(2000);

    // 後退
    ledcWrite(PIN_1, centerDuty - step);
    ledcWrite(PIN_2, centerDuty + step);
    delay(2000);

    // 左旋回
    ledcWrite(PIN_1, centerDuty - step);
    ledcWrite(PIN_2, centerDuty - step);
    delay(2000);

    // 右旋回
    ledcWrite(PIN_1, centerDuty + step);
    ledcWrite(PIN_2, centerDuty + step);
    delay(2000);

    // 停止
    ledcWrite(PIN_1, centerDuty);
    ledcWrite(PIN_2, centerDuty);

    run = false;
  }
}
```
