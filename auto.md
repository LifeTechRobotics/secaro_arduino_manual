---
# Page settings
layout: default
keywords:
comments: false

# Hero section
title: 自律走行
description: ロボットを自律走行させてみましょう。

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
**Arduinoの導入**ー**サンプルプログラムの動かし方** を参照し、Atom Lite を起動します。

利用するサンプルプロジェクトファイル： auto\auto.ino

※サンプルプログラムは、**[Github](https://github.com/LifeTechRobotics/secaro_arduino_projects.git)** よりダウンロードしてください。

## サンプルプログラムができること
ロボットが自律走行します。7cm 以内に障害物が検知された場合、自動で右へ方向変換して回避します。

回避中は最小速度で走行します。
