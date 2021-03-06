---
title: take operator-Azure データエクスプローラー |Microsoft Docs
description: この記事では、Azure データエクスプローラーでの take 演算子について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/13/2020
ms.openlocfilehash: a57dd8cde9ea00b0b68ae95ff557bd3b530357cc
ms.sourcegitcommit: 3dfaaa5567f8a5598702d52e4aa787d4249824d4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/05/2020
ms.locfileid: "87804153"
---
# <a name="take-operator"></a>take 演算子

指定された行数まで返します。

```kusto
T | take 5
```

ソースデータを並べ替える場合を除き、どのレコードが返されるかは保証されません。

> [!NOTE]
> `take`は、データを対話的に参照しているときに小さなサンプルレコードを表示するためのシンプルで迅速かつ効率的な方法ですが、データセットが変更されていない場合でも、複数回実行しても結果の一貫性が保証されないことに注意してください。
> クエリによって返される行の数がクエリによって明示的に制限されていない (演算子が使用されていない) 場合でも `take` 、Kusto はその数値を既定で制限します。 詳細については、「 [Kusto クエリの制限](../concepts/querylimits.md)」を参照してください。

## <a name="syntax"></a>構文

`take`*Numberofrows* 
 `limit`*Numberofrows*

( `take` と `limit` はシノニムです)。

## <a name="does-kusto-support-paging-of-query-results"></a>Kusto はクエリ結果のページングをサポートしていますか?

Kusto には、組み込みのページングメカニズムが用意されていません。

Kusto は、大量のデータセットに対する優れたクエリパフォーマンスを提供するために格納されるデータを継続的に最適化する、複雑なサービスです。 ページングはリソースが限られているステートレスクライアントに便利なメカニズムですが、クライアントの状態情報を追跡する必要があるバックエンドサービスに負荷を移すことになります。 その後、バックエンドサービスのパフォーマンスとスケーラビリティは厳しく制限されています。

ページングをサポートするには、次の機能のいずれかを実装します。

* クエリの結果を外部ストレージにエクスポートし、生成されたデータをページングします。

* Kusto クエリの結果をキャッシュすることによって、ステートフルなページング API を提供する中間層アプリケーションを作成する。

## <a name="see-also"></a>関連項目

* [sort 演算子](sortoperator.md)
* [top 演算子](topoperator.md)
* [top-nested 演算子](topnestedoperator.md)