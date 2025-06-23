---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: 自律走行
description: ロボットが障害物を避けられるようにしてみましょう。

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
        content: ラジコン操作
        url: /remote
    next:
        content: 今後について
        url: /future
---

# サンプルプログラムを動かしてみる
**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照し、Atom Lite を起動します。

利用するサンプルプロジェクトファイル： auto\auto.ino

※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。

## サンプルプログラムができること
ロボットが前進中に、7cm 以内に障害物が検知された場合、自動で右へ方向変換して回避します。

回避中は最小速度で走行します。

# プログラミング
## setup()
各種初期化を行います。

前章より障害物を検知するための TOF センサーサーボの初期化が追加されます。

GPIO ピン番号 23 を利用します。

ロボット前進中に TOF センサーは 30°〜150° の範囲内で 500ms おきに 30° ずつ回転します。

```
#define PIN_SS 23              // センサーサーボ

// センサーサーボの回転角度
#define DEGREE_MIN 30
#define DEGREE_MAX 150

#define INTERVAL_TIME_SS 500         // センサーサーボの回転間隔
#define OBSTACLE_THRESHOLD 70        // 障害探知距離（mm）

int deg_step = 30;                   // 1回につきの回転角度

void setup() {
    // センサーサーボのPIN設定
    sensorServo.attach(PIN_SS);
    sensorServo.write(DEGREE_MIN);

    // TOFセンサーの初期化
    sensor.setBus(&Wire);
    sensor.setTimeout(TIME_OUT_SENSOR);
    if (!sensor.init()) {
      Serial.println("Failed to initialize sensor.\n");
    } else {
      sensor.setDistanceMode(VL53L1X::Short);
      sensor.setMeasurementTimingBudget(50000);
      sensor.startContinuous(50); // 測定間隔
    }
}
```

## loop()
7cm 以内に障害物が検知された場合、ロボットが回避モードに入り、回避モード中では、進行方向が一時的に右旋回に変更され、最小速度で 5000ms 走行します。
その後、回避モードが解消され、進行方向と速度が回避前に戻ります。
