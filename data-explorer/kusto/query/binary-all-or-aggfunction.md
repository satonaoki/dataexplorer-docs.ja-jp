---
title: binary_all_or () (集計関数)-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーの binary_all_or () (集計関数) について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/24/2020
ms.openlocfilehash: 35478b435814fe716f7130576c16714403490be6
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87349127"
---
# <a name="binary_all_or-aggregation-function"></a>binary_all_or () (集計関数)

概要作成グループごとにバイナリ演算を使用して値を累積し `OR` ます (または、グループ化せずに概要作成を実行する場合は合計)。

* [集計の](summarizeoperator.md)コンテキストでのみ使用できます。

## <a name="syntax"></a>構文

集計の `binary_all_or(` *Expr*`)`

## <a name="arguments"></a>引数

* *Expr*: long 数値。

## <a name="returns"></a>戻り値

集計グループごとのレコードに対するバイナリ演算を使用して集計された値を返し `OR` ます (集約がグループ化されていない場合は合計で)。

## <a name="example"></a>例

バイナリ演算を使用して ' カフェ ' を作成してい `OR` ます:

<!-- csl: https://help.kusto.windows.net/Samples -->
```kusto
datatable(num:long)
[
  0x88888008,
  0x42000000,
  0x00767000,
  0x00000005, 
]
| summarize result = toupper(tohex(binary_all_or(num)))
```

|結果|
|---|
|CAFEF00D|
