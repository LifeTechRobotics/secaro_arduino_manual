---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Arduinoの導入
description: Arduinoを導入するための環境設定を行います。

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
    # prev:
    #     content: Previous page
    #     url: '#'
    next:
        content: 構成要素
        url: /component
---

# 利用環境
- Windows 11
- Atom Lite
- Arduino 2.3.6

## Arduino
### Arduino のインストール
1. <https://www.arduino.cc/en/software/#ide> にアクセスし、 Arduino IDE をダウンロードします。 Windows 64bit OS のため、今回は **Windows Win 10 and newer, 64 bits** を選択します。

    <div class="callout callout--info">
        <p>ダウンロードするにはメールアドレスが必要です。</p>
    </div>
    ![1](../images/enviroment/1.png)

2. ダウンロードしたファイル (*arduino-ide_2.3.6_Windows_64bit.exe*) をダブルクリックし、実行します。
    ![201](../images/enviroment/201.png)
    ![202](../images/enviroment/202.png)
    ![203](../images/enviroment/203.png)
    ![204](../images/enviroment/204.png)
    ![205](../images/enviroment/205.png)


### ボードのインストール
1. ファイル > **基本設定** を開きます。
    ![3](../images/enviroment/3.png)

    <div class="callout callout--info">
        <p><strong>M5Stack ボード管理 URL</strong></p>
        <p>https://static-cdn.m5stack.com/resource/arduino/package_m5stack_index.json</p>
    </div>
    
    設定 タブの一番下の 追加のボードマネージャのURL 項目に上記の **M5Stack ボード管理 URL** を入力し、**OK** をクリックして保存します。

    ![4](../images/enviroment/4.png)

2. サイドバーの**ボード管理**を開き、**M5Stack** を入力し、**インストール** をクリックします。
    ![5](../images/enviroment/5.png)

3. 使用するボードに応じて、ツール > ボード > M5Stack > 対応する開発ボードを選択します。Atom Lite の場合は **M5Atom** を選択します。
    ![6](../images/enviroment/6.png)

### ライブラリのインストール
1. サイドバーの**ライブラリ管理**を開き、**M5Atom** を入力し、**インストール** をクリックします。
    ![7](../images/enviroment/7.png)

    **全てをインストール** をクリックします。
    ![8](../images/enviroment/8.png)

### Arduino プログラムの動かし方
1. Atom Lite を USB で PC に接続します。

2. Auduino IDE で ファイル > **開く** をクリックし、サンプルプログラム (.ino ファイル) を選択します。
    ![9](../images/enviroment/9.png)

    ![10](../images/enviroment/10.png)

3. ボードを **M5Atom** に指定します。![12](../images/enviroment/upload.png) アイコンをクリックし、コンパイルされた後、Atom Lite へ書き込みます。
    ![13](../images/enviroment/verify.png) はデバイスへ書き込まずコンパイルのみが実施されます。 
    ![11](../images/enviroment/11.png)

### Arduino プログラムの基本構造
Arduinoで書くプログラムは「スケッチ」と呼ばれ、基本的にこの2つの関数から構成されます。
```
void setup() {
  // 最初に一度だけ実行される処理
}

void loop() {
  // 繰り返し実行される処理
}
```
