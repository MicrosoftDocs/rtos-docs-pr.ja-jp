---
title: 付録 C - DOS コマンド ライン ユーティリティ
description: Azure RTOS TraceX のインストールでは、Utilities サブディレクトリ下に 3 つの DOS コマンド ライン ユーティリティがあります。
author: philmea
ms.service: rtos
ms.topic: article
ms.date: 5/19/2020
ms.author: philmea
ms.openlocfilehash: 1a89f8be7e21e416659b904f0ec5b2a3a8f666cdb9a861786e652a38564db48f
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2021
ms.locfileid: "116791552"
---
# <a name="appendix-c---dos-command-line-utilities"></a>付録 C - DOS コマンド ライン ユーティリティ

TraceX のインストールでは、***Utilities*** サブディレクトリ下に 3 つの DOS コマンド ライン ユーティリティがあります。

提供されるユーティリティの一覧を次に示します。

| **ユーティリティ**                              | **目的**                               | **コマンド ライン定義** |
| -------------------------------- | ----------------------------------------- | ---------------------------- |
| **ea2tracex.exe**                | ThreadX によって original_file の関連付けで GHS ツール converted_file を使用して生成されたトレース ファイル ea2tracex を TraceX トレース ファイル形式に変換します。 GHS 向け ThreadX ツールでは、非 GHS 向け ThreadX ツールとは異なるトレース形式が生成されるため、この変換ユーティリティが必要になります。 | ``` > eatracex original_file converted_file <cr> ``` | 
**hex2tracex.exe** | ThreadX によって生成されたが、開発ツールから Intel HEX 形式でダンプされたトレース ファイルを、バイナリ TraceX トレース ファイル形式に変換します。 TraceX V5 以降では、変換せずに HEX ファイルを開くことができます。 | ``` hex2tracex hex_file converted_file <cr> ``` | 
**mot2tracex.exe** | ThreadX によって生成されたが、開発ツールから Motorola S-Record 形式でダンプされたトレース ファイルを、バイナリ TraceX トレース ファイル形式に変換します。 TraceX V5 以降では、変換せずに S-Record ファイルを開くことができます。 | ``` > mot2tracex mot_file converted_file <cr> ```|