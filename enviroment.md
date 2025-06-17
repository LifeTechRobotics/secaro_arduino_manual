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
    ![101](../images/enviroment/101.png)

    無料版をダウンロードします。
    ![102](../images/enviroment/102.png)

    メールアドレスを入力し、同意事項にチェックを入れて、購読なしでダウンロードします。
    ![103](../images/enviroment/103.png)

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

### サンプルプログラムの動かし方

#### ファームウェアを書き込む
1. Atom Lite を USB で PC に接続します。

    <div class="callout callout--info">
        <strong><p>環境によって FTDI ドライバーが必要な場合</p></strong>
        <p>下記のダウンロード先より取得してください。</p>
    </div>
    <https://ftdichip.com/drivers/vcp-drivers/> にアクセスし、ドライバーをダウンロードします。ダウンロードした CDM2123620_Setup.zip ファイルを展開し、インストールします。
    ![9](../images/enviroment/9.png)

2. Auduino IDE で ファイル > **開く** をクリックし、プロジェクトファイル (.ino ファイル) を選択します。

    ※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/developwitharduino_projects)** よりダウンロードしてください。

    ![10](../images/enviroment/10.png)

    ![11](../images/enviroment/11.png)

3. ボードは自動的に **M5Atom** として認識されます。![12](../images/enviroment/upload.png) アイコンをクリックし、コンパイル後、Atom Lite へ書き込まれます。
    <div class="callout callout--danger">
        <p><strong>Atom Lite が動く可能性があるので、ご注意ください。</strong></p>
        <p>USB 接続中では通電状態なので、ファームウェアによって書き込む直後に Atom Lite が動く時があります。</p>
        <p>ケーブルが絡むと故障の可能性があるので、書き込むアイコンを押す前に、ロボットの車輪が浮くよう、手で持ち上げてください。</p>
    </div>

    ![14](../images/enviroment/14.png)

    <div class="callout callout--info">
        <strong><p>M5Atom のボード名でポート番号が複数ある場合</p></strong>
        <p>デバイスマネージャーで USB Serial Port 番号を確認してください。</p>
    </div>
    コントロールバネル > **デバイスマネージャー** を開きます。
    ![15](../images/enviroment/15.png)

#### Atom Lite を動かす
1. USB ケーブルを外します。

2. Atom Lite の電源ボタンを入れます。
    <div class="callout callout--danger">
        <p><strong>Atom Lite が動く時があるので、ご注意ください。</strong></p>
        <p>ファームウェアによって電源を入れた直後に Atom Lite が動く時があります。</p>
        <p>ロボットが動いても大丈夫な場所を確保してから行ってください。</p>
    </div>

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
Atom Lite の電源を入れると、最初に setup() が一度だけ実行され、その後、繰り返し loop() 処理が実行されます。