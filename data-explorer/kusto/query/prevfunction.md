---
title: prev ()-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーの prev () について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/13/2020
ms.openlocfilehash: fb781834d77aee678103a14714721ff0d46f7b3a
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87346101"
---
# <a name="prev"></a>prev()

指定された行の特定の列の値を返します。
指定された行は、シリアル化された[行セット](./windowsfunctions.md#serialized-row-set)の現在の行の指定されたオフセット位置にあります。

## <a name="syntax"></a>構文

いくつかの可能性があります。

* `prev(column)`

* `prev(column, offset)`

* `prev(column, offset, default_value)`

## <a name="arguments"></a>引数

* `column`: 値の取得元の列。

* `offset`: 行に戻るオフセット。 オフセットが指定されていない場合、既定のオフセット1が使用されます。

* `default_value`: 値を取得する前の行がない場合に使用される既定値。 既定値が指定されていない場合、null が使用されます。

## <a name="examples"></a>使用例

```kusto
Table | serialize | extend prevA = prev(A,1)
| extend diff = A - prevA
| where diff > 1

Table | serialize prevA = prev(A,1,10)
| extend diff = A - prevA
| where diff <= 10
```
