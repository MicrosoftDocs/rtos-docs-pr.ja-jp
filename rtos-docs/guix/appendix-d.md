---
title: 別表 D - GUIX のブラシ、キャンバス、グラデーションの属性
description: GUIX のブラシ、キャンバス、グラデーションの属性について説明します。
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 9fbc98f1094cab6be4bc0826fef7c0feb77b50b066b22342cd52404bd85ff98e
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2021
ms.locfileid: "116784633"
---
# <a name="appendix-d---guix-brush-canvas-and-gradient-attributes"></a>別表 D - GUIX のブラシ、キャンバス、グラデーションの属性

__**ブラシ スタイル:**__

**GX_BRUSH_OUTLINE**
- 値: 0x0000
- 説明: このブラシ スタイルは、gx_canvas_rectangle_draw や gx_canvas_polygon_draw などの図形描画の関数に適用されます。 このスタイルでは図形の輪郭が描画され、必要ならば塗りつぶすこともできます。 GX_BRUSH_OUTLINE スタイルを設定して GX_BRSUH_SOLID_FILL を設定しない場合、図形の輪郭だけが描画されます。

**GX_BRUSH_SOLID_FILL**
- 値: 0x0001
- 説明: このブラシ スタイルは図形描画の関数に適用され、現在のブラシ塗りつぶしの色で、要求された図形を塗りつぶすことを示します。

**GX_BRUSH_PIXELMAP_FILL**
- 値: 0x0002
- 説明: このブラシ スタイルは図形描画の関数に適用され、現在のブラシ pixelmap (ピクセルマップ) のパターンで、要求された図形を塗りつぶすことを示します。

**GX_BRUSH_ALIAS**
- 値: 0x0004
- 説明: このブラシ スタイルは、すべての線描と図形の輪郭に適用されます。 このフラグを設定するとアンチエイリアス描画アルゴリズムが適用され、描画が正確になる代わりにかかる時間が長くなります。 このスタイル フラグは 16 bpp 以上の色深度でのみ使用されます。

**GX_BRUSH_UNDERLINE**
- 値: 0x0008
- 説明: このフラグはテキストの描画に適用され、フラグ設定後に描画するテキストに下線を付けることを示します。

**GX_BRUSH_ROUND**
- 値: 0x0010
- 説明: このフラグは線描に適用され、線の端が既定の角ばった形状ではなく円い形状を示します。

__**キャンバスのフラグ:**__

**GX_CANVAS_SIMPLE**
- 値: 0x01
- 説明: 画面に表示されない描画に使用されるメモリ キャンバスです。

**GX_CANVAS_MANAGED**
- 値: 0x02
- 説明: 合成作成プロセスの一環として、または単一キャンバス アーキテクチャのバッファー切り替え操作の一環として、アクティブな画面に自動的にフラッシュされるキャンバス。

**GX_CANVAS_VISIBLE**
- 値: 0x04
- 説明: このフラグを使用すれば、キャンバスの描画内容を失わずにキャンバスをオンまたはオフできます。

**GX_CANVAS_MODIFIED**
- 値: 0x08
- 説明: 将来使用するために確保されています。

**GX_CANVAS_COMPOSITE**
- 値: 0x20
- 説明: アプリケーションはこのフラグを使用して、管理している複数のキャンバスを合成キャンバス上に合成する複数キャンバス システムを構成し、合成したものをハードウェアのフレーム バッファーに送ります。

__**グラデーションの種類:**__

**GX_GRADIENT_TYPE_VERTICAL**
- 値: 0x01
- 説明: アルファマップの垂直方向のグラデーションを作成します。

**GX_GRADIENT_TYPE_ALPHA**
- 値: 0x02
- 説明: アルファマップ スタイルのグラデーションを作成します。 これは、現在サポートされている唯一のグラデーション スタイルです。

**GX_GRADIENT_TYPE_MIRROR**
- 値: 0x04
- 説明: このフラグは、幅や高さの範囲の中央でグラデーションが最大になり、右端や下端に達すると最初の値に戻ることを示します。 このスタイル フラグを設定しない場合、GX_GRADIENT_TYPE_VERTICAL フラグに応じて、グラデーションは上から下、または左から右の線形グラデーションになります。