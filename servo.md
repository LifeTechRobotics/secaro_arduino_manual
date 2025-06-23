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

**Arduinoの導入** ー **サンプルプログラムの動かし方** を参照し、Atom Lite を起動します。

- 利用するサンプルプロジェクトファイル：`servo/servo.ino`
- サンプルプログラムは、**[GitHub](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。

## サンプルプログラムができること

片方の車輪が下記のように動作します。

1. ゆっくり加速しながら正転 → ゆっくり減速しながら正転 → 停止  
2. ゆっくり加速しながら逆転 → ゆっくり減速しながら逆転 → 停止  

---

# プログラミング

## setup()

各種初期化を行います。

```cpp
void setup() {
    // シリアル通信の初期化
    Serial.begin(115200);
    delay(100);

    // M5の初期化
    M5.begin(true, false, false);

    // LEDC PIN設定
    ledcAttach(PIN_1, FREQ, RESOLUTION);

    // 初期停止
    ledcWrite(PIN_1, centerDuty);
    delay(1000);
}
```

### ledcAttach関数

この関数は、指定した GPIO ピンに PWM 出力（LED 調光、モーター制御など）を割り当て、内部で自動的にタイマーとチャンネルをセットアップします。

#### 基本的な使い方

```cpp
bool ledcAttach(uint8_t pin, uint32_t freq, uint8_t resolution);
```

- `pin`：PWM 信号を出力する GPIO 番号（例：13 や 25）。本サンプルでは 19 を使用。  
- `freq`：PWM の周波数（Hz）。例：50（サーボ用）、5000（LED制御など）。今回は 50Hz を指定。  
- `resolution`：PWM の分解能（bit単位）。例：8bit（256段階）、12bit（4096段階）。今回のモーター制御では 12bit を使用。

---

## loop()

速度制御を行います。

```cpp
ledcWrite(PIN_1, duty);
```

### ledcWrite関数

この関数は、ESP32などのマイコンで PWM（パルス幅変調）を使って LED やモーターなどを制御するために使います。

#### 基本的な使い方

```cpp
void ledcWrite(uint8_t pin, uint32_t duty);
```

- `pin`：PWM 信号を出力する GPIO 番号（例：13 や 25）。本サンプルでは 19 を使用。  
- `duty`：デューティー比（PWM の ON 時間）。値の範囲は、事前に設定した `resolution` に依存します。

#### 事前の初期化が必要

`ledcWrite()` を使う前に、以下のように初期設定を行っておく必要があります。

```cpp
#define PIN_1 19          // 車輪サーボ1
#define FREQ 50           // PWM周波数（Hz）
#define RESOLUTION 12     // 分解能（12ビット）

void setup() {
    ledcAttach(PIN_1, FREQ, RESOLUTION);
}
```

---

## デューティー比について

今回使用する Servo Kit 360° の仕様：

- 周波数：50Hz（周期 = 20ms）  
- パルス幅：0.5ms〜2.5ms（中立は1.5ms）

### 分解能の設定

PWMの周期 20ms をどれだけ細かく分けられるかは分解能に依存します。  
たとえば 12bit（2¹² = 4096）に設定すると：

```yaml
PWM周期 = 4096ステップ
1ステップ ≒ 4.88μs（20ms ÷ 4096）
1.5ms = 約307ステップ
2.5ms = 約512ステップ
0.5ms = 約102ステップ
```

### 回転とデューティー比の関係

このサーボモーターは「連続回転型サーボ」です。PWM のデューティー比（≒パルス幅）により、以下のように動作します：

- 約 307：中立（1.5ms）→ 停止  
- 約 102：最小値（0.5ms）→ 最大逆転  
- 約 512：最大値（2.5ms）→ 最大正転  

- **正転範囲**：約 307〜512  
- **逆転範囲**：約 307〜102

---
