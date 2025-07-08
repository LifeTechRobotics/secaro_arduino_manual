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
        content: 5. ラジコン操作
        url: /remote
    next:
        content: 7. ROSについて
        url: /ros
---

# サンプルプログラムを動かしてみる

**Arduino IDEの導入 > <a href="../enviroment/#%E3%82%B5%E3%83%B3%E3%83%97%E3%83%AB%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%A0%E3%81%AE%E5%8B%95%E3%81%8B%E3%81%97%E6%96%B9" target="_blank" rel="noopener noreferrer">サンプルプログラムの動かし方</a>** を参照し、Atom Lite を起動します。

- 利用するサンプルプロジェクトファイル：`auto/auto.ino`  
- サンプルプログラムは、**<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードしてください。

## サンプルプログラムができること

ロボットが前進中に、**7cm 以内に障害物が検知された場合**、自動で右へ方向変換して回避します。回避中は**最小速度で走行**します。

---

# プログラミング

## setup()

各種初期化を行います。  
前章と比べて、障害物検知用の TOF センサーとセンサー用サーボの初期化が追加されます。  
センサー用サーボには GPIO ピン 23 を使用します。

TOF センサーは、ロボット前進中に **30°〜150°** の範囲内で、**500ms ごとに 30°ずつ**回転します。

```cpp
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
        Serial.println("Failed to initialize sensor.\\n");
    } else {
        sensor.setDistanceMode(VL53L1X::Short);
        sensor.setMeasurementTimingBudget(50000);
        sensor.startContinuous(50); // 測定間隔
    }
}
```

## loop()

**7cm 以内に障害物が検知された場合**、ロボットは**回避モード**に入ります。

- 回避モード中は、一時的に右旋回に切り替え  
- **最小速度で 5000ms 走行**  
- その後、回避モードが解除され、元の進行方向と速度に復帰します

```cpp
void loop() {
  if (command == 'F') {
    distance = sensor.read();
    if (distance > 0 && distance < OBSTACLE_THRESHOLD) { // 障害物が検知された
      Serial.println("Obstacle detected.\n");

      // 回避モード
      obstacleFlg = true;

      // 回避前の速度と指令を退避
      spdL_taihi = spdL;
      spdR_taihi = spdR;
      command_taihi = command;

      // 回避中は最小速度
      spdL = 1;
      spdR = 1;

      // 右回転して回避
      command = 'R';

      // 回避必要な時間
      avoidTimeNeed = 5000; //暫定

      // 回避開始時間
      avoidTimeStart = millis();
    } else {
      // 前進
      ledcWrite(PIN_1, centerDutyHigh + step*spdL);
      ledcWrite(PIN_2, centerDutyLow - step*spdR);

      currentMillis = millis();
      if (previousMillis == 0) { // 初回の場合
        previousMillis = currentMillis;
      }
      // 前進時のみセンサーを動かす
      if ((currentMillis - previousMillis) >= INTERVAL_TIME_SS) {
        // センサーサーボの角度を設定
        if (ccw) {
          deg += deg_step;
          if (deg >= DEGREE_MAX) {
            deg = DEGREE_MAX;
          }
        } else {
          deg -= deg_step;
          if (deg < DEGREE_MIN) {
            deg = DEGREE_MIN;
          }
        }
        sensorServo.write(deg);
        previousMillis = millis();

        // 次回の回転方向を決める
        if (deg >= DEGREE_MAX) {
          ccw = false;
        } else if (deg <= DEGREE_MIN) {
          ccw = true;
        }
      }
    }
  } else if (command == 'R') {
    if (obstacleFlg) { // 回避モード
      avoidTimeCurrent = millis();
      if ((avoidTimeCurrent - avoidTimeStart) >= avoidTimeNeed) { // 回避終了
        obstacleFlg = false;

        // 回避前の速度に戻す
        spdL = spdL_taihi;
        spdR = spdR_taihi;

        // 回避前の指令に戻す
        command = command_taihi;

        // 回避用変数初期化
        avoidTimeStart = 0;
        avoidTimeCurrent = 0;
        avoidTimeNeed = 0;
        spdL_taihi = 1;
        spdR_taihi = 1;
        command_taihi = '\0';        
      } else {
        // 右旋回
        ledcWrite(PIN_1, centerDutyHigh + step*spdL);
        ledcWrite(PIN_2, centerDutyHigh + step*spdR);
      }
    } else {
      // 右旋回
      ledcWrite(PIN_1, centerDutyHigh + step*spdL);
      ledcWrite(PIN_2, centerDutyHigh + step*spdR);
    }
}

```