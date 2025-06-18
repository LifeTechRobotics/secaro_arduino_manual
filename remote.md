---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: ラジコン操作
description: ロボットを遠隔操作できるようにしてみます。

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
        content: ロボット動作
        url: /robot
    next:
        content: 自律走行
        url: /auto
---

# 利用環境
- Windows 11
- Python 3.8
- Kivy 2.3.1

## 必要なライブラリ
- pyserial
- pybluez

## インストール方法
PC 左下の検索欄に **shell** を入力し、最も一致する検索結果の **Windows PowerShell** をクリックします。
![0](../images/remote/0.png)

下記コマンドを順次に実行します。
```
python -m pip install --upgrade pip
pip install kivy
pip install pyserial
pip install pybluez
```
pybluez がインストールエラーの場合以下を試す：
```
pip install git+https://github.com/pybluez/pybluez.git#egg=pybluez
```
# リモート操作前の準備
## ラジコン操作画面を立ち上げる
1. サンプルプログラムの controller フォルダを開き、右クリックしてメニューの**ターミナルで開く**をクリックします。

   ※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。
 

2. ターミナルウィンドウで下記コマンドを実行します。
    ```
    python .\controller.py
    ```
3. リモート操作画面が表示されます。
    ![1](../images/remote/1.png)

## Atom Lite を立ち上げる
サンプルプロジェクトファイル： remote\remote.ino

実行方法は、**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照してください。

<div class="callout callout--info">
    <p><strong>ラジコン操作画面で操作しない限り、Atom Lite が動きません。</strong></p>
</div>

※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。

# リモート操作してみる
## ロボットへ接続する
1. リモート画面で**デバイスをスキャン**ボタンをクリックし、ロボットを探します。

    **スキャン中**が表示され、暫くしたら、検知された Bluetooth デバイスの一覧が表示されます。
    ![2](../images/remote/2.png)

2. 一覧に名前が **Nemueee(xx:xx:xx:xx:xx:xx)** のデバイスをクリックし、ペアリングの指示に従って手動でペアリングを行います。ペアリング完了まで OK ボタンを押さないでください。
    ![3](../images/remote/3.png)

3. PC > 設定 > **Bluetooth とデバイス** を開き、**デバイスの追加**より Bluetooth 種類を選択し、同じ名前 Nemueee のデバイスを追加します。

    ![4](../images/remote/4.png)

    ![5](../images/remote/5.png)

    ![6](../images/remote/6.png)

    ![7](../images/remote/7.png)

    デバイス Nemueee の状態が**ペアリング済み**になったら、ペアリング完了です。
    ![8](../images/remote/8.png)

4. リモート操作画面で **OK** ボタンをクリックします。デバイス Nemueee の右側のステータスが**接続済み**になったら、ロボットを操作可能になります。
    ![9](../images/remote/9.png)

## ロボットを動かす
### 速度指定
速度スライダーで左右の車輪の速度をそれぞれに調整できます。1 ～ 9 段階を指定可能。
![10](../images/remote/slider.png)

### 進行方向指定
- 前進　　![11](../images/remote/up.png)
- 後退　　![11](../images/remote/down.png)
- 左旋回　![11](../images/remote/left.png)
- 右旋回　![11](../images/remote/right.png)


### 停止
真ん中の停止ボタンをクリックしたら、ロボットが停止します。

![11](../images/remote/stop.png)
