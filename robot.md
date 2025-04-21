---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: ロボット動作
description: ロボットを前後左右に動かしてみます。

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

# ロボットを動かす

## サンプルプログラムを動かしてみる
プロジェクトファイル： robot.ino

実行方法は、**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照してください。

### サンプルプログラムができること
ロボットが **前進** > **後退** > **左旋回** > **右旋回** > **停止** の順で動きます。

## プログラミング
### setup()
M5 Atom Lite の初期化を行います。
```
// M5 の初期化
M5.begin(true, false, true);
```
ledcWrite() を使う前に、PWM のチャンネル設定やピンの割り当てを行います。
```
const int ledPin1 = 19;       // Servo1 PIN 19 を使用
const int ledPin2 = 22;       // Servo2 PIN 22 を使用
const int pwmChannel1 = 1;    // チャンネル 1 を使う
const int pwmChannel2 = 2;    // チャンネル 2 を使う

// PWM チャンネルの初期化    
ledcSetup(pwmChannel1, freq, resolution);
ledcSetup(pwmChannel2, freq, resolution);
// チャンネルとピンの紐づけ
ledcAttachPin(ledPin1, pwmChannel1);
ledcAttachPin(ledPin2, pwmChannel2);
```
- サーボ Pin 19は、 チャンネル 1 と紐つけます。
- サーボ Pin 22は、 チャンネル 2 と紐つけます。

### loop()
速度の設定を行います。

今回利用するサーボモータが設定可能なデューティ比の範囲：
- 5000 〜 8500 正回転
    - 最小値 5000
    - 最大値 8500
- 5000 〜 1500 逆回転
    - 最小値 5000
    - 最大値 1500

```
#define DUTY_LOW 1500
#define DUTY_MID 5000
#define DUTY_HIGH 8500
#define DUTY_STEP 1000

void loop() {
  if(run == true) {
    //前進
    ledcWrite(pwmChannel1, DUTY_MID + DUTY_STEP);
    ledcWrite(pwmChannel2, DUTY_MID - DUTY_STEP);
    delay(2000);

    //後退
    ledcWrite(pwmChannel1, DUTY_MID - DUTY_STEP);
    ledcWrite(pwmChannel2, DUTY_MID + DUTY_STEP);
    delay(2000);
    
    //左旋回
    ledcWrite(pwmChannel1, DUTY_MID - DUTY_STEP);
    ledcWrite(pwmChannel2, DUTY_MID - DUTY_STEP);
    delay(2000);

    //右旋回
    ledcWrite(pwmChannel1, DUTY_MID + DUTY_STEP);
    ledcWrite(pwmChannel2, DUTY_MID + DUTY_STEP);
    delay(2000);

    // 停止
    ledcWrite(pwmChannel1, 0);
    ledcWrite(pwmChannel2, 0);

    run = false;
  }
}
```
- 前進
    - サーボモータ 1    正回転
    - サーボモータ 2    逆回転
- 後退
    - サーボモータ 1    逆回転
    - サーボモータ 2    正回転
- 左旋回
    - サーボモータ 1    逆回転
    - サーボモータ 2    逆回転
- 右旋回
    - サーボモータ 1    正回転
    - サーボモータ 2    正回転
