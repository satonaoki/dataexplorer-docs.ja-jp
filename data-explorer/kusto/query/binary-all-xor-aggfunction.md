---
title: binary_all_xor () (集計関数)-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーの binary_all_xor () (集計関数) について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 03/06/2020
ms.openlocfilehash: c2fab7abbcebdc97f6f1c394f840bf6e232db03a
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87349110"
---
# <a name="binary_all_xor-aggregation-function"></a>binary_all_xor () (集計関数)

概要作成グループごとにバイナリ演算を使用して値を累積し `XOR` ます (または、グループ化せずに概要作成を実行する場合は合計)。

* [集計の](summarizeoperator.md)コンテキストでのみ使用できます。

## <a name="syntax"></a>構文

集計の `binary_all_xor(` *Expr*`)`

## <a name="arguments"></a>引数

* *Expr*: long 数値。

## <a name="returns"></a>戻り値

集計グループごとのレコードに対するバイナリ演算を使用して集計された値を返し `XOR` ます (集約がグループ化されていない場合は合計で)。

## <a name="example"></a>例

バイナリ演算を使用して ' カフェ ' を作成してい `XOR` ます:

<!-- csl: https://help.kusto.windows.net/Samples -->
```kusto
datatable(num:long)
[
  0x44404440,
  0x1E1E1E1E,
  0x90ABBA09,
  0x000B105A,
]
| summarize result = toupper(tohex(binary_all_xor(num)))
```

|結果|
|---|
|CAFEF00D|
