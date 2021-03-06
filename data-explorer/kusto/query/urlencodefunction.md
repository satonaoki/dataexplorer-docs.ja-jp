---
title: url_encode ()-Azure データエクスプローラー |Microsoft Docs
description: この記事では、Azure データエクスプローラーの url_encode () について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 03/17/2020
ms.openlocfilehash: 8ccc93286073003bdaf8324611888d32f60910fb
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87350572"
---
# <a name="url_encode"></a>url_encode()

関数は、入力 URL の文字をインターネット経由で送信できる形式に変換します。 

URL エンコードとデコードの詳細については、[こちら](https://en.wikipedia.org/wiki/Percent-encoding)を参照してください。
は、スペースを ' 20% ' ではなく ' + ' としてエンコードすることによって[url_encode_component](./urlencodecomponentfunction.md)と[は](https://en.wikipedia.org/wiki/Percent-encoding)異なります (「application/url エンコード」を参照してください)。

## <a name="syntax"></a>構文

`url_encode(`*先*`)`

## <a name="arguments"></a>引数

* *url*: 入力 url (文字列)。  

## <a name="returns"></a>戻り値

URL (文字列) をインターネット経由で送信できる形式に変換します。

## <a name="examples"></a>例

```kusto
let url = @'https://www.bing.com/hello word';
print original = url, encoded = url_encode(url)
```

|original|化|
|---|---|
|https://www.bing.com/helloテキスト|https %3 a %2 f %2 f www. bing .com% 2fhello + word|


 