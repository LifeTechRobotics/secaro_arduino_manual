---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: Arduino IDEの導入
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
        content: 2. 構成要素
        url: /component
---

# 利用環境
- Windows 11  
- Atom Lite  
- Arduino 2.3.6  


# Arduino IDEの導入

## Arduino IDEのインストール

1. <a href="https://www.arduino.cc/en/software/#ide" target="_blank" rel="noopener noreferrer">https://www.arduino.cc/en/software/#ide</a> にアクセスし、Arduino IDE をダウンロードします。自身のPC環境を確認し、適切なものを選択します。今回は例として、**Windows Win 10 and newer, 64 bits** を選択します。  
    ![101](../images/enviroment/101.png)

    **JUST DOWNLOAD** をクリックします。  
    ![102](../images/enviroment/102.png)  

    ![103](../images/enviroment/103.png)

2. ダウンロードしたファイル（*arduino-ide_2.3.6_Windows_64bit.exe*）をダブルクリックして実行します。  
    ![201](../images/enviroment/201.png)  

    ![202](../images/enviroment/202.png)  

    ![203](../images/enviroment/203.png)  

    ![204](../images/enviroment/204.png)  
    
    ![205](../images/enviroment/205.png)


### ボードのインストール

使用するデバイスに応じて、必要なボードをインストールします。
本マニュアルでは Atom Lite を使用するため、対応するボードのインストールを行います。

1. メニューから **ファイル > 基本設定** を開きます。  
    ![3](../images/enviroment/3.png)

    > 💡 **M5Stack ボード管理 URL**  
    > https://static-cdn.m5stack.com/resource/arduino/package_m5stack_index.json

    「追加のボードマネージャの URL」に上記を入力し、**OK** をクリックして保存します。  
    ![4](../images/enviroment/4.png)

2. サイドバーの **ボードマネージャー** を開き、`M5Stack` を検索して **インストール** をクリックします。  
    ![5](../images/enviroment/5.png)

3. 使用するボードに応じて、**ツール > ボード > M5Stack** から該当する開発ボードを選択します。Atom Lite の場合は **M5Atom** を選択してください。  
    ![6](../images/enviroment/6.png)


### ライブラリのインストール

本マニュアルでは Atom Lite を使用するため、必要な関連ライブラリをインストールします。

1. サイドバーの **ライブラリマネージャー** を開き、`M5Atom` を検索して **インストール** をクリックします。  
    ![7](../images/enviroment/7.png)

    **全てをインストール** をクリックします。  
    ![8](../images/enviroment/8.png)


## サンプルプログラムの動かし方

### ファームウェアを書き込む

1. Atom Lite を USB で PC に接続します。

    > ℹ️ **FTDI ドライバが必要な場合**  
    > 使用しているパソコンによっては、FTDI ドライバのインストールが必要になる場合があります。  
    > ※通常の Windows 11 であれば、標準ドライバで動作するため下記手順は必要ありません。  
    > Atom Lite と接続出来ない場合などに試してみてください。  
    
    > <a href="https://ftdichip.com/drivers/vcp-drivers" target="_blank" rel="noopener noreferrer">https://ftdichip.com/drivers/vcp-drivers</a> より `CDM2123620_Setup.zip` をダウンロード・展開・インストールしてください。  
    > ![9](../images/enviroment/9.png)

2. Arduino IDE で **ファイル > 開く** をクリックし、プロジェクトファイル（`.ino`）を選択します。  

    ※サンプルプログラムは **<a href="https://github.com/LifeTechRobotics/secaro_arduino_projects.git" target="_blank" rel="noopener noreferrer">GitHub</a>** からダウンロード可能です。  
    ![10](../images/enviroment/10.png)  

    ![11](../images/enviroment/11.png)

3. ボードが自動で **M5Atom** として認識されない場合、**他のボードとポートを選択** をクリックして、  
    - ボード: **M5Atom**  
    - ポート: **COM? Serial Port (USB)**  
    を選択してください。  
    ![12](../images/enviroment/1201.png)  

    ![12](../images/enviroment/1202.png)

4. ![13](../images/enviroment/upload.png) をクリックして書き込みます。  

    <div class="callout callout--danger">
        <p><strong>注意：ロボットがすぐ動く場合があります</strong></p>
        <p>書き込み直後にロボットが動くため、ケーブルが絡まないよう、書き込む前にロボットの車輪が浮くように手で持ち上げてください。</p>
    </div>
    
    ![14](../images/enviroment/14.png)

    > ℹ️ **複数のポートがある場合**  
    > デバイスマネージャーからポート番号を確認してください。  
    > **コントロールパネル** > **デバイスマネージャー**  
    > ![15](../images/enviroment/15.png)


### ロボットを動かす

1. USB ケーブルを外します。  
2. ロボットの電源ボタンを ON にします。

    <div class="callout callout--danger">
        <p><strong>注意：ロボットがすぐ動く場合があります</strong></p>
        <p>電源を入れた直後にロボットが動くことがありますので、動いても問題ない安全な場所で行ってください。</p>
    </div>


## Arduinoプログラムの基本構造

Arduino で書くプログラム（スケッチ）は、主に次の 2 つの関数から構成されます。

```cpp
void setup() {
  // 最初に一度だけ実行される処理
  // xxの初期化
}

void loop() {
  // 繰り返し実行される処理
}
```
