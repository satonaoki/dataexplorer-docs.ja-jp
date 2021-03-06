---
title: hll_merge () (集計関数)-Azure データエクスプローラー |Microsoft Docs
description: この記事では、Azure データエクスプローラーの hll_merge () (集計関数) について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 04/15/2019
ms.openlocfilehash: 4681f92155181f85cad5c46ed70a79cb173d437f
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87347546"
---
# <a name="hll_merge-aggregation-function"></a>hll_merge () (集計関数)

`HLL`グループ全体の結果を1つの `HLL` 値にマージします。

* 集計のコンテキスト内でのみ使用できます[。](summarizeoperator.md)

詳細については、[基になるアルゴリズム (*H*Yper*l*og*l*og) と推定精度](dcount-aggfunction.md#estimation-accuracy)に関する説明を参照してください。

## <a name="syntax"></a>構文

`summarize``hll_merge(` *Expr*`)`

## <a name="arguments"></a>引数

* `*Expr*`: 集計計算に使用される式。

## <a name="returns"></a>戻り値

関数は、グループ全体ののマージされた値を返し `hll` `*Expr*` ます。
 
**ヒント**

1) 関数 [dcount_hll] (dcount-hllfunction.md) を使用して、 `dcount` 集計関数からを計算し `hll`  /  `hll-merge` ます。
