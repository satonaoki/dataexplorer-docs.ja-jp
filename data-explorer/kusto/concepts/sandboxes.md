---
title: サンドボックス - Azure データ エクスプローラー |マイクロソフトドキュメント
description: この記事では、Azure データ エクスプローラーのサンドボックスについて説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 03/30/2020
ms.openlocfilehash: faa7a8225aecd5992e3064fd10cfcc0e559c5127
ms.sourcegitcommit: 47a002b7032a05ef67c4e5e12de7720062645e9e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2020
ms.locfileid: "81522969"
---
# <a name="sandboxes"></a>サンドボックス

Kusto の Data Engine サービスは、安全な分離を必要とする特定のフローを実行するためのサンドボックスを実行できます。
これらのフローの例は[、Python プラグイン](../query/pythonplugin.md)または[R プラグイン](../query/rplugin.md)を使用して実行されるユーザー定義スクリプトです。

これらのサンドボックスを実行するために、Kustoはマイクロソフトの[Drawbridge](https://www.microsoft.com/research/project/drawbridge/)プロジェクトの進化を使用しています。 このソリューションは、マルチテナント環境でユーザー定義オブジェクトを実行するために、他の Microsoft サービスによって使用されます。

サンドボックス内で実行されるフローは分離されるだけでなく、ローカル (データに近い) であるため、リモート呼び出しに追加された待機時間はありません。

## <a name="prerequisites"></a>前提条件

* データ エンジンで[ディスク暗号化](https://docs.microsoft.com/azure/data-explorer/security#data-encryption)を有効**にすることはできません**。
  * 両方の機能を並行して実行するためのサポートは、将来的に期待されています。
* サンドボックスを実行するために必要なパッケージ(イメージ)は、Data Engineの各ノードに展開され、実行するには専用のSSDスペースが必要です。
  * 推定サイズは 20 GB で、たとえば、D14_v2 VM の SSD 容量は約 2.5%、L16_v1 VM の SSD 容量は 0.7% です。
  * これはクラスターのデータ容量に影響するため、クラスターの[コスト](https://azure.microsoft.com/pricing/details/data-explorer)に影響を与える可能性があります。

## <a name="runtime"></a>ランタイム

* サンドボックス化されたクエリ演算子は、サンドボックスの実行に 1 つ以上のサンドボックスを使用できます。
  * これらは、1 回の実行にのみ使用され、複数の実行で共有されず、実行が完了すると破棄されます。
* 各ノードは、受信要求を実行できる事前に定義されたサンドボックスの量を保持します。
  * サンドボックスが使用されると、新しいサンドボックスが自動的に割り当てられ、置き換えられます。
* クエリ演算子を提供するために事前に割り当てられたサンドボックスがない場合は、調整されます。
  (新しいサンドボックスが割り当てられるまでは、「[エラー](#errors)」を参照してください (SKU とデータノードの使用可能なリソースによっては、サンドボックスごとに最大 10 ~ 15 秒かかる可能性があります)。

## <a name="limitations"></a>制限事項

これらの制限の一部は、サンドボックスの種類ごとにクラスターレベルの[サンドボックスポリシー](../management/sandboxpolicy.md)を使用して制御できます。

* **ノードあたりのサンドボックス数:** ノードあたりのサンドボックス数は制限されています。
  * 使用可能なサンドボックスがない状態に遭遇した要求は、調整されます。
* **ネットワーク:** サンドボックスは、VM 上のリソースや VM の外部にあるリソースと対話できません。
  * サンドボックスは別のサンドボックスと対話できません。
* **CPU:** サンドボックスがホストのプロセッサに消費できる CPU の最大速度は制限されます (デフォルトでは`50%`)。
  * この制限に達すると、サンドボックスの CPU 使用率は調整されますが、実行は続行されます。
* **メモリ:** サンドボックスがホストの RAM を消費できる RAM の最大量は制限されます (`20GB`デフォルトでは )。
  * この制限に達すると、サンドボックスが終了します (およびクエリ実行エラー)。
* **ディスク:** サンドボックスには一意のディレクトリがアタッチされており、ホストのファイルシステムにアクセスすることはできません。
  * このフォルダは、サンドボックスの種類(カスタマイズできない Python または R パッケージなど)に一致するイメージ/パッケージへのアクセスを提供します。
* **子プロセス:** サンドボックスは子プロセスの生成をブロックされます。

> [!NOTE]
> Sandbox のリソース使用率は、要求の一部として処理されるデータのサイズだけでなく、サンドボックス内で実行されるロジックと、そのサンドボックスで使用されるライブラリの実装によっても異なります。
> (例えば`python`、プラグインと`r`プラグインの場合、後者は、ユーザーが提供するスクリプトと、実行時に使用するPythonまたはRライブラリを意味します)

## <a name="errors"></a>エラー

|コード                      |Message                                                                                        |潜在的な理由                                                                                                    |
|--------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
|E_SB_QUERY_THROTTLED_ERROR|制限付きクエリは、調整のため中止されました。 バックオフ後の再試行は成功する可能性があります。   |ターゲット ノードに使用可能なサンドボックスがありません。 新しいサンドボックスは数秒で利用可能になるはずです         |
|E_SB_QUERY_THROTTLED_ERROR|種類 '{kind}' のサンドボックスがまだ初期化されていません                                       |サンドボックス ポリシーが最近変更されました。 新しいポリシーに従う新しいサンドボックスは数秒で利用可能になります|