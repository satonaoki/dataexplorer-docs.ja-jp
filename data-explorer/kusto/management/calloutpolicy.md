---
title: コールアウトポリシー-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーのコールアウトポリシーについて説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 04/01/2020
ms.openlocfilehash: 809088f35567f85444755d89ab30e02fad46abaf
ms.sourcegitcommit: 313a91d2a34383b5a6e39add6c8b7fabb4f8d39a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/16/2020
ms.locfileid: "90680679"
---
# <a name="callout-policy"></a>Callout ポリシー

Azure データエクスプローラークラスターは、さまざまなシナリオで外部サービスと通信できます。
クラスター管理者は、クラスターのコールアウトポリシーを更新することによって、外部呼び出しに対して承認されたドメインを管理できます。

コールアウトポリシーはクラスターレベルで管理され、次の種類に分類されます。
* `kusto` -Azure データエクスプローラークロスクラスタークエリを制御します。
* `sql` - [SQL プラグイン](../query/sqlrequestplugin.md)を制御します。
* `cosmosdb` - [CosmosDB プラグイン](../query/cosmosdb-plugin.md)を制御します。
* `webapi` -他の外部 Web 呼び出しを制御します。
* `sandbox_artifacts`-サンドボックスプラグイン ([python](../query/pythonplugin.md)  |  [R](../query/rplugin.md)) を制御します。
* `external_data` - [外部テーブル](../query/schema-entities/externaltables.md) または [externaldata](../query/externaldata-operator.md) 演算子を使用して外部データへのアクセスを制御します。

コールアウトポリシーは、次の要素で構成されます。

* **CalloutType** -コールアウトの種類を定義します。 `kusto` 、、またはにすることができます。 `sql``webapi`
* **CalloutUriRegex** -コールアウトのドメインで許可されている Regex を指定します。
* **Cancall** -コールアウトに外部呼び出しが許可されているかどうかを示します。

## <a name="predefined-callout-policies"></a>定義済みのコールアウトポリシー

この表は、すべての Azure データエクスプローラークラスターに事前に構成されている一連の定義済みコールアウトポリシーを示しています。これにより、すべてのサービスを選択できます。

|サービス      |クラウド        |役職  |許可されるドメイン |
|-------------|-------------|-------------|-------------|
|Kusto |`Public Azure` |クロスクラスタークエリ |`^[^.]*\.kusto\.windows\.net$` <br> `^[^.]*\.kustomfa\.windows\.net$` |
|Kusto |`Black Forest` |クロスクラスタークエリ |`^[^.]*\.kusto\.cloudapi\.de$` <br> `^[^.]*\.kustomfa\.cloudapi\.de$` |
|Kusto |`Fairfax` |クロスクラスタークエリ |`^[^.]*\.kusto\.usgovcloudapi\.net$` <br> `^[^.]*\.kustomfa\.usgovcloudapi\.net$` |
|Kusto |`Mooncake` |クロスクラスタークエリ |`^[^.]*\.kusto\.chinacloudapi\.cn$` <br> `^[^.]*\.kustomfa\.chinacloudapi\.cn$` |
|Azure DB |`Public Azure` |SQL 要求 |`^[^.]*\.database\.windows\.net$` <br> `^[^.]*\.databasemfa\.windows\.net$` |
|Azure DB |`Black Forest` |SQL 要求 |`^[^.]*\.database\.cloudapi\.de$` <br> `^[^.]*\.databasemfa\.cloudapi\.de$` |
|Azure DB |`Fairfax` |SQL 要求 |`^[^.]*\.database\.usgovcloudapi\.net$` <br> `^[^.]*\.databasemfa\.usgovcloudapi\.net$` |
|Azure DB |`Mooncake` |SQL 要求 |`^[^.]*\.database\.chinacloudapi\.cn$` <br> `^[^.]*\.databasemfa\.chinacloudapi\.cn$` |
|基準サービス |パブリック Azure |ベースライン要求 |`baseliningsvc-int.azurewebsites.net` <br> `baseliningsvc-ppe.azurewebsites.net` <br> `baseliningsvc-prod.azurewebsites.net` |

## <a name="control-commands"></a>管理コマンド

コマンドには [Alldatabasesadmin](access-control/role-based-authorization.md) アクセス許可が必要です。

**構成済みのすべてのコールアウトポリシーを表示する**

```kusto
.show cluster policy callout
```

**コールアウトポリシーの変更**

```kusto
.alter cluster policy callout @'[{"CalloutType": "webapi","CalloutUriRegex": "en\\.wikipedia\\.org","CanCall": true}]'
```

**許可された一連のコールアウトを追加する**

```kusto
.alter-merge cluster policy callout @'[{"CalloutType": "webapi","CalloutUriRegex": "en\\.wikipedia\\.org","CanCall": true}, {"CalloutType": "webapi","CalloutUriRegex": "bing\\.com","CanCall": true}]'
```

**変更できないコールアウトポリシーをすべて削除します。**

```kusto
.delete cluster policy callout
```
