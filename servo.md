---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: サーボモータ動作
description: サーボモータで車輪を動かしてみます。

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
        content: 構成要素
        url: /component
    next:
        content: ロボット動作
        url: /robot
---

# サンプルプログラムを動かしてみる
プロジェクトファイル： servo.ino

実行方法は、**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照してください。

## サンプルプログラムができること
片方の車輪だけを動かします。

速度を上げながら**正回転** > **停止** > 速度を上げながら**逆回転** > **停止** の順で動きます。

# プログラミング
## ledcWrite関数
ledcWrite(uint8_t chan, uint32_t duty) は、ESP32などのマイコンボードで PWM（パルス幅変調）を使って LED やモーターなどを制御するための関数です。Arduino IDE や PlatformIO などで ESP32 開発するときによく使われます。

### 基本的な使い方
```
ledcWrite(チャンネル番号, デューティー比);
```
- chan: 設定した PWM チャンネル番号（0〜15の範囲）。

- duty: デューティー比（PWMのON時間）。範囲は、事前に設定したビット数（例えば 8ビットなら 0〜255）に依存します。

### 事前の初期化が必要
ledcWrite() を使う前に、以下のように PWM のチャンネル設定やピンの割り当てをしておく必要があります。

例：
```
const int ledPin = 18;       // GPIO 18 を使用
const int pwmChannel = 0;    // チャンネル 0 を使う
const int freq = 5000;       // PWM 周波数
const int resolution = 8;    // 8ビットの分解能（0～255）

void setup() {
  // PWM チャンネルの初期化
  ledcSetup(pwmChannel, freq, resolution);

  // チャンネルとピンの紐づけ
  ledcAttachPin(ledPin, pwmChannel);
}

void loop() {
  // デューティー比を 127（約50%）に設定
  ledcWrite(pwmChannel, 127);
  delay(1000);

  // デューティー比を 255（100%）に設定
  ledcWrite(pwmChannel, 255);
  delay(1000);

  // デューティー比を 0（OFF）に設定
  ledcWrite(pwmChannel, 0);
  delay(1000);
}
```
###  ポイント
- resolution を 8 にすれば、duty は 0〜255 の範囲。
- resolution を 10 にすれば、duty は 0〜1023。
- ledcWrite() は、アナログのような出力（PWM）をデジタルピンで実現できます。

## setup()
M5 Atom Lite の初期化を行います。
```
// M5 の初期化
M5.begin(true, false, true);
```
ledcWrite() を使う前に、PWM のチャンネル設定やピンの割り当てを行います。
```
const int ledPin = 19;       // Servo PIN 19 を使用
const int pwmChannel = 1;    // チャンネル 1 を使う

// PWM チャンネルの初期化
ledcSetup(pwmChannel, freq, resolution);
// チャンネルとピンの紐づけ
ledcAttachPin(ledPin, pwmChannel);
}
```
- チャンネルは、1　を利用します。
- サーボ Pin は、 19 を利用します。

## loop()
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

void loop() {
  if(run == true) {
    //正回転
    for(int i = DUTY_MID; i < DUTY_HIGH; i = i + 100){  
      ledcWrite(pwmChannel, i);
      delay(50);
    }

    // 停止
    ledcWrite(pwmChannel, 0);
    delay(1000);
    
    //逆回転
    for(int i = DUTY_MID; i > DUTY_LOW; i = i - 100){  
      ledcWrite(pwmChannel, i);
      delay(50);
    }
    
    // 停止
    ledcWrite(pwmChannel, 0);

    run = false;
  }
}
```
