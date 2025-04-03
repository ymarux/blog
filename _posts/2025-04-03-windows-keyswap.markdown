---
layout: post
title:  "Windows で F1 キー無効化と CapsLock を左 Ctrl キーに入れ替える"
date:   2025-04-03 11:04:00 +0900
categories: Windows
---
# 前提

Windows のみ。
Windows11 まで確認済み。

# 手順

- regedit.exe を起動
- 以下に移動
```
\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout
```
- 新規 → バイナリ値 で 「Scancode Map」を作成。
- 以下を入力
```
00 00 00 00 00 00 00 00
03 00 00 03 1D 00 3A 00
00 00 3B 00 00 00 00 00
```

| コード | 説明 |
| --- | --- |
| 00 00 00 00 | ヘッダー：バージョン。すべて 0 にする。 |
| 00 00 00 00 | ヘッダー：フラグ。すべて 0 にする。 |
| 03 00 00 03 | 0x03000000 にエントリ数を加算。null 終端含む。 |
| 1D 00 3A 00 | 0x3A00（CapsLock）→  0x1D00（Ctrl）|
| 00 00 3B 00 | 0x3B00（F1）→ 0x0000（null） |
| 00 00 00 00 | null 終端 |

- OSを再起動する

## 参考
https://docs.microsoft.com/ja-jp/windows-hardware/drivers/hid/keyboard-and-mouse-class-drivers
