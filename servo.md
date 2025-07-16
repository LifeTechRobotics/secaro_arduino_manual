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
        content: 2. 構成要素
        url: /component
    next:
        content: 4. ロボット動作
        url: /robot
---

# サンプルプログラム

## サンプルプログラムのダウンロード

- 利用するサンプルプロジェクトファイル：`servo/servo.ino`
- サンプルプログラムは、**<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** よりダウンロードしてください。

## サンプルプログラムができること

左の車輪は、次の順序で動作します。

1. ゆっくり加速しながら正転 → ゆっくり減速しながら正転 → 停止  
2. ゆっくり加速しながら逆転 → ゆっくり減速しながら逆転 → 停止  

## 走行動画

<div style="padding:75% 0 0 0;position:relative;"><iframe src="https://player.vimeo.com/video/1101540493?badge=0&amp;autopause=0&amp;player_id=0&amp;app_id=58479" frameborder="0" allow="autoplay; fullscreen; picture-in-picture; clipboard-write; encrypted-media; web-share" style="position:absolute;top:0;left:0;width:100%;height:100%;" title="VID_20250715_135442133"></iframe></div><script src="https://player.vimeo.com/api/player.js"></script>

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

- **360度連続回転サーボモータ**  
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


## loop()

速度制御を行います。

```cpp
ledcWrite(PIN_1, duty);
```

### ledcWrite関数

この関数は、ESP32 などのマイコンで PWM（パルス幅変調）を使って LED やモーターなどを制御するために使います。

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


## デューティー比について

今回使用する Servo Kit 360° の仕様：

- 周波数：50Hz（周期 = 20ms）  
- パルス幅：0.5ms〜2.5ms（中立は1.5ms）

### 分解能の設定

PWM の周期 20ms をどれだけ細かく分けられるかは分解能に依存します。  
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

