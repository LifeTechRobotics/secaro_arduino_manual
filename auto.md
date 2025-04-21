---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: 自律走行
description: お掃除ロボットのように、走行中に前方に障害物がある場合、方向転換をするようにしてみます。

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
        content: ラジコン操作
        url: /remote
    next:
        content: 今後について
        url: /future
---

# 障害物を自動で避けられるようにする

## サンプルプログラムを動かしてみる
プロジェクトファイル： Auto_ATOMLite.m5f

実行方法は、**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照してください。

### リモート操作してみる

### サンプルプログラムができること
- 速度調整スライダー
- 前進　　↑
- 後退　　↓
- 左旋回　←
- 右旋回　→
- 停止　　○

速度を設定した後、前進／後退／左旋回／右旋回をタップしてロボットを動かします。

真ん中の停止ボタンをタップしてロボットを停止します。

走行中に障害物が5cm以内に検知された場合、ロボットが自動で方向を変えます。

## プログラミング
### setup()
### loop()