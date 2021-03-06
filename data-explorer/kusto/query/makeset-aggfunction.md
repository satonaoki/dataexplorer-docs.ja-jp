---
title: make_set () (集計関数)-Azure データエクスプローラー |Microsoft Docs
description: この記事では、Azure データエクスプローラーの make_set () (集計関数) について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 01/23/2020
ms.openlocfilehash: c85738928aa65bf2a4476f10afa065c2a8ca1faf
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87346917"
---
# <a name="make_set-aggregation-function"></a>make_set () (集計関数)

グループ内にある*式*で使用される個別の値セットの `dynamic` (JSON) 配列を返します 

* [集計の](summarizeoperator.md)コンテキストでのみ使用できます。

## <a name="syntax"></a>構文

`summarize``make_set(` *Expr* [ `,` *MaxSize*]`)`

## <a name="arguments"></a>引数

* *Expr*: 集計計算用の式。
* *MaxSize*は、返される要素の最大数に対する整数制限 (省略可能) です (既定値は*1048576*)。 MaxSize 値は1048576を超えることはできません。

> [!NOTE]
> この関数の従来型および旧形式のバリアントでは、 `makeset()` *MaxSize* = 128 の既定の制限が設定されています。

## <a name="returns"></a>戻り値

グループ内にある*式*で使用される個別の値セットの `dynamic` (JSON) 配列を返します 
配列の並べ替え順序が定義されていません。

> [!TIP]
> 個別の値をカウントするだけの場合は、 [dcount ()](dcount-aggfunction.md)を使用します。

## <a name="example"></a>例

```kusto
PageViewLog 
| summarize countries=make_set(country) by continent
```

:::image type="content" source="images/makeset-aggfunction/makeset.png" alt-text="Makeset":::

**参照**

* [`mv-expand`](./mvexpandoperator.md)反対の関数には演算子を使用します。
* [`make_set_if`](./makesetif-aggfunction.md)演算子はに似 `make_set` ていますが、述語も受け入れる点が異なります。