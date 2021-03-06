---
title: estimate_data_size ()-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーの estimate_data_size () について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/13/2020
ms.openlocfilehash: 00438528f00c91a95fde9a77a7aa18a75002fce9
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87348243"
---
# <a name="estimate_data_size"></a>estimate_data_size()

表形式の式で選択されている列の推定データサイズ (バイト単位) を返します。

```kusto
estimate_data_size(*)
estimate_data_size(Col1, Col2, Col3)
```

## <a name="syntax"></a>構文

`estimate_data_size(*)`

`estimate_data_size(`*col1* `, `*col2* `, `...`)`

## <a name="arguments"></a>引数

* *col1*, *col2*: データサイズの推定に使用されるソース表形式式の列参照の選択。 すべての列を含めるには、 `*` (アスタリスク) 構文を使用します。

## <a name="returns"></a>戻り値

* レコードサイズの推定データサイズ (バイト単位)。 推定は、データ型と値の長さに基づいています。

## <a name="examples"></a>例

合計データサイズの計算に使用する `estimated_data_size()` もの:

<!-- csl: https://help.kusto.windows.net/Samples -->
```kusto
range x from 1 to 10 step 1                    // x (long) is 8 
| extend Text = '1234567890'                   // Text length is 10  
| summarize Total=sum(estimate_data_size(*))   // (8+10)x10 = 180
```

|合計|
|---|
|180|
