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
        content: サーボモータ動作
        url: /servo
    next:
        content: ラジコン操作
        url: /remote
---

# サンプルプログラムを動かしてみる

**Arduinoの導入** ー **サンプルプログラムの動かし方** を参照し、Atom Lite を起動します。

- 利用するサンプルプロジェクトファイル：`robot/robot.ino`  
- サンプルプログラムは、**[GitHub](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。

## サンプルプログラムができること

ロボットが下記のように動作します：

1. 前進  
2. 後退  
3. 左旋回  
4. 右旋回  
5. 停止  

---

# プログラミング

## setup()

各種初期化を行います。  
2つの車輪を動かすため、前章よりも使用する GPIO ピンが一つ増えます。ここでは GPIO 19 と 22 を利用します。

```cpp
#define PIN_1 19          // 車輪サーボ1
#define PIN_2 22          // 車輪サーボ2

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
