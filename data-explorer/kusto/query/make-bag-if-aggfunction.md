---
title: make_bag_if () (集計関数)-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーの make_bag_if () (集計関数) について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/13/2020
ms.openlocfilehash: 67e408653a4873dce3b5e8f21a91775573affbe2
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87347019"
---
# <a name="make_bag_if-aggregation-function"></a>make_bag_if () (集計関数)

述語が `dynamic` に評価される、グループ内の *' Expr '* のすべての値の (JSON) プロパティバッグ (ディクショナリ*Predicate* ) を返し `true` ます。

> [!NOTE]
> 集計のコンテキストでのみ使用でき[ます。](summarizeoperator.md)

## <a name="syntax"></a>構文

`summarize``make_bag_if(` *Expr*、*述語*[ `,` *MaxSize*]`)`

## <a name="arguments"></a>引数

* *Expr*: `dynamic` 集計計算に使用される型の式です。
* *述語*: `true` 結果に *' Expr '* を追加するために、に評価する必要がある述語。
* *MaxSize*: 返される要素の最大数に対する整数の制限 (省略可能) (既定値は*1048576*)。 MaxSize 値は1048576を超えることはできません。

## <a name="returns"></a>戻り値

`dynamic`*述語*がに評価されるプロパティバッグ (ディクショナリ) である、グループ内の *' Expr '* のすべての値の (JSON) プロパティバッグ (ディクショナリ) を返し `true` ます。
ディクショナリ以外の値はスキップされます。
1つのキーが複数の行に表示されている場合は、このキーに指定できる値から任意の値が選択されます。

> [!NOTE]
> 関数は、 [`make_bag`](./make-bag-aggfunction.md) 述語式のない make_bag_if () に似ています。

## <a name="examples"></a>例

```kusto
let T = datatable(prop:string, value:string, predicate:bool)
[
    "prop01", "val_a", true,
    "prop02", "val_b", false,
    "prop03", "val_c", true
];
T
| extend p = pack(prop, value)
| summarize dict=make_bag_if(p, predicate)

```

|dict|
|----|
|{"prop01": "val_a", "prop03": "val_c"} |

Make_bag_if () 出力のバッグキーを列に変換するには、 [bag_unpack ()](bag-unpackplugin.md)プラグインを使用します。 

```kusto
let T = datatable(prop:string, value:string, predicate:bool)
[
    "prop01", "val_a", true,
    "prop02", "val_b", false,
    "prop03", "val_c", true
];
T
| extend p = pack(prop, value)
| summarize bag=make_bag_if(p, predicate)
| evaluate bag_unpack(bag)

```

|prop01|prop03|
|---|---|
|val_a|val_c|
