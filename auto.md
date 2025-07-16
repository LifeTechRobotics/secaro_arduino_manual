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

# サンプルプログラム

## サンプルプログラムのダウンロード

- 利用するサンプルプロジェクトファイル：`auto/auto.ino`
- サンプルプログラムは、**<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードしてください。


## サンプルプログラムができること

ロボットが前進中に、**7cm以内に障害物** を検知した場合、自動的に右方向へ進路を変更し、回避動作を行います。  
回避中は、安全のため **最小速度** で走行します。  

また、ラジコン操作と同様に、走行方向および速度はリモートで制御することが可能です。  

## 走行動画

<div style="padding:56.25% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1101543109?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="VID_20250715_142819590"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

# 自律走行の前準備

前章「ラジコン操作」と同様の操作画面を使用して、ロボットの自律走行を開始・停止するために、事前に以下の準備を行ってください。

👉 **ラジコン操作 > [リモート操作前の準備](../remote/#%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88%E6%93%8D%E4%BD%9C%E5%89%8D%E3%81%AE%E6%BA%96%E5%82%99){:target="_blank" rel="noopener noreferrer"}**


# 自律走行させてみる

ラジコン操作と同様の操作画面を使って、ロボットの自律走行を制御します。  
まずは以下の手順でロボットとの接続を行ってください。

👉 **ラジコン操作 > [ロボットに接続する](../remote/#%E3%83%AD%E3%83%9C%E3%83%83%E3%83%88%E3%81%AB%E6%8E%A5%E7%B6%9A%E3%81%99%E3%82%8B){:target="_blank" rel="noopener noreferrer"}**

## 走行開始

「前進」ボタンを押すと、ロボットの自律走行が開始されます。  
  ![1](../images/auto/up.png)

## 走行終了

「停止」ボタンを押すと、ロボットの走行が停止します。  
  ![1](../images/auto/stop.png)

# プログラミング

## 依存関係

このスケッチを正常に動作させるためには、以下のハードウェアおよびライブラリが必要です。


### 使用ハードウェア

- **M5Atom Lite（ESP32ベース）**  
  M5Stack 社製の小型マイコン。Bluetooth 通信機能と PWM 制御機能を持つ ESP32 を搭載しています。

- **360度連続回転サーボモータ**  
  PWM 信号のデューティ比によって回転速度と方向を制御できるタイプのサーボモータです。

- **標準サーボモータ × 1（センサー旋回用）**  
  TOF センサーを左右に動かす用途です。

- **TOF距離センサー（VL53L1X）**  
  4cm から 400cm の範囲で、I2C 接続のレーザー距離センサーです。


### 必要ライブラリ

- **M5Atom ライブラリ**  
  M5.begin() など、M5Atom シリーズ向けの機能を提供します。  
  ※**Arduino IDE** でインストールしておく必要があります。(**Arduino IDEの導入 > <a href="../enviroment/#%E3%83%A9%E3%82%A4%E3%83%96%E3%83%A9%E3%83%AA%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB" target="_blank" rel="noopener noreferrer">ライブラリのインストール</a>** )
  
- **esp32-hal-ledc.h**  
  ESP32 の PWM（LEDC）制御に必要なヘッダファイルです。  
  ※Arduino Core for ESP32 に標準で含まれており、別途インストールは不要です。

- **ESP32Servo**  
  標準サーボモータ制御ライブラリ（sensorServo 用）。  
  ※**Arduino IDE** の **ライブラリマネージャ** からインストール可能です。  

    サイドバーの **ライブラリマネージャー** を開き、検索欄に `ESP32Servo` を入力し、**インストール** をクリックします。  
      ![1](../images/auto/1.png)

- **VL53L1X**  
  TOF 距離センサー用ライブラリ。Pololu 製。  
  ※**Arduino IDE** の **ライブラリマネージャ** からインストール可能です。  

    サイドバーの **ライブラリマネージャー** を開き、検索欄に `VL53L1X` を入力します。  
    複数のライブラリが表示されますが、VL53L1X `by Pololu` を選び、 **インストール** をクリックします。  
      ![1](../images/auto/2.png)

### ピン設定

| 機能              | GPIOピン |
|-------------------|----------|
| サーボモータ1     | 19       |
| サーボモータ2     | 22       |
| センサーサーボ    | 23       |
| I2C SDA（VL53L1X）| 26       |
| I2C SCL（VL53L1X）| 32       |


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

**7cm 以内に障害物が検知された場合**、ロボットは **回避モード** に入ります。

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