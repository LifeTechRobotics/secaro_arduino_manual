---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: サーボモータ動作
description: サーボモータで車輪を動かしてみましょう。

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
**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照し、Atom Lite を起動します。

利用するサンプルプロジェクトファイル： servo\servo.ino

※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。

## サンプルプログラムができること
片方の車輪だけを動かします。

速度を上げながら**正回転** > **停止** > 速度を上げながら**逆回転** > **停止** の順で動きます。

# プログラミング
## setup()
各種初期化を行います。

## loop()
速度の設定を行います。


## ledcWrite関数
この関数は、ESP32などのマイコンボードで PWM（パルス幅変調）を使って LED やモーターなどを制御するための関数です。Arduino IDE や PlatformIO などで ESP32 開発するときによく使われます。

### 基本的な使い方
```
void ledcWrite(uint8_t pin, uint32_t duty);
```
- pin: PWM 信号を出力する GPIO 番号（例：13 や 25）。本章では 19 を利用します。

- duty: デューティー比（PWMのON時間）。範囲は、事前に設定した分解能（resolution）に依存します。

### 事前の初期化が必要
ledcWrite() を使う前に、以下のようにピンの割り当てをしておく必要があります。

```
void setup() {
  // LEDC PIN設定
  ledcAttach(PIN_1, FREQ, RESOLUTION);
}
```
## ledcAttach関数
この関数は、指定した GPIO ピンに対して PWM 出力（LED 調光、モーター制御など）を割り当て、内部で自動的にタイマーとチャンネルをセットアップします。

### 基本的な使い方
```
bool ledcAttach(uint8_t pin, uint32_t freq, uint8_t resolution);
```
- pin: PWM 信号を出力する GPIO 番号（例：13 や 25）。本章では 19 を利用します。

- freq: PWM の周波数（Hz）。例：50（サーボ）、5000（LED）。今回利用する Servo Kit 360° の標準仕様は 50Hz。

- resolution: 分解能（bit単位）。例：8bit（256段階）、12bit（4096段階）

## デューティー比
今回利用する Servo Kit 360° の仕様：
- 周波数： 50Hz（周期 = 20ms）
- パルス幅： 0.5ms〜2.5ms（中心は 1.5ms）

パルス幅を 0.5ms〜2.5ms の範囲で制御するには：

20ms を何ステップで分けるか = 分解能に依存

たとえば 16bit（2^16 = 65535） の場合：
```yaml
PWM周期 = 65535ステップ → 1ステップ ≈ 0.3μs（= 20ms / 65535）
1.5ms = 約4915ステップ
2.5ms = 約8191ステップ
0.5ms = 約1638ステップ
```
よって、今回利用する Servo Kit 360° のデューティー比範囲は、約1638〜8191。

また、このサーボモーターは「連続回転型サーボ」です。

PWM 信号の デューティ比（≒パルス幅）によって、回転方向・回転速度を制御します。

中立値（1.5ms）で停止、0.5ms で最大逆転、2.5ms で最大正転。

すなわち、正転範囲は、約4915〜8191、逆転範囲は、約4915〜1638。

