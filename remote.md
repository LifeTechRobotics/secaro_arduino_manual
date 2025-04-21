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

# 遠隔でロボットを操作する

## サンプルプログラムを動かしてみる
プロジェクトファイル： remote.ino

実行方法は、**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照してください。

### リモート操作してみる

### サンプルプログラムができること
- 速度調整スライダー
- 前進　　↑
- 後退　　↓
- 左旋回　←
- 右旋回　→
- 停止　　○

速度を設定してから、前進／後退／左旋回／右旋回をタップしてロボットを動かします。

真ん中の停止ボタンをタップしてロボットを停止します。

## プログラミング
### setup()
### loop()