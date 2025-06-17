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

# サンプルプログラムを動かしてみる
サンプルプロジェクトファイル： auto\auto.ino

実行方法は、**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照してください。

※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/developwitharduino_projects)** よりダウンロードしてください。

## サンプルプログラムができること
ロボットが自律走行します。7cm 以内に障害物が検知された場合、自動で右へ方向変換して回避します。

回避中は最小速度で走行します。
