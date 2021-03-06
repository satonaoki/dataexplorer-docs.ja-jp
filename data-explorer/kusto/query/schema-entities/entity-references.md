---
title: エンティティ参照-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーでのエンティティ参照について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 02/13/2020
ms.openlocfilehash: 9d652ea8551a21d542ad6afef575616e7387183f
ms.sourcegitcommit: 4eb64e72861d07cedb879e7b61a59eced74517ec
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/29/2020
ms.locfileid: "85517888"
---
# <a name="entity-references"></a>エンティティ参照

クエリの名前を使用して Kusto スキーマエンティティを参照します。 有効なエンティティ名には、*データベース*、*テーブル*、*列*、および格納されている関数が含まれます。 *クラスター*名でクラスターを参照することはできません。
エンティティのコンテナーが現在のコンテキストで明確でない場合は、追加の資格がなくてもエンティティ名を使用します。 たとえば、というデータベースに対してクエリを実行する場合、 `DB` そのデータベースでという名前のテーブルをという名前で参照することができ `T` `T` ます。

エンティティのコンテナーがコンテキストから使用できない場合、またはコンテキストでコンテナーとは異なるコンテナーからエンティティを参照する場合は、エンティティの**修飾名**を使用します。
名前は、コンテナーのにエンティティ名を連結したものであり、コンテナーのなどになる可能性があります。 このようにして、データベースに対して実行されるクエリは、を使用して、 `DB` `T1` 同じクラスターの別のデータベース内のテーブルを参照することがあり `DB1` `database("DB1").T1` ます。 クエリで別のクラスターのテーブルを参照する場合は、などを使用し `cluster("https://C2.kusto.windows.net/").database("DB2").T2` ます。

エンティティ参照では、エンティティのコンテナーのコンテキスト内で一意である限り、エンティティ名を使用することもできます。 詳細については、「[エンティティの名前](./entity-names.md#entity-pretty-names)」を参照してください。

## <a name="wildcard-matching-for-entity-names"></a>エンティティ名のワイルドカードの照合

一部のコンテキストでは、ワイルドカード () を使用して、 `*` エンティティ名の全体または一部を一致させることができます。 たとえば、次のクエリでは、現在のデータベースのすべてのテーブルと、その名前がで始まるデータベース内のすべてのテーブルを参照してい `DB` `T` ます。

```kusto
union *, database("DB1").T*
```

> [!NOTE]
> ワイルドカードの照合は、ドル記号 () で始まるエンティティ名と一致することはできません `$` 。
このような名前はシステムによって予約されています。

## <a name="next-steps"></a>次のステップ

* [スキーマエンティティ型](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/)
* [スキーマエンティティ名](https://docs.microsoft.com/azure/data-explorer/kusto/query/schema-entities/entity-names)
