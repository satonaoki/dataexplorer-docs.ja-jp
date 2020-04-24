---
title: base64_decode_toarray() - Azure データ エクスプローラー |マイクロソフトドキュメント
description: この記事では、Azure データ エクスプローラーでbase64_decode_toarray() について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 06/22/2019
ms.openlocfilehash: 80a702f112fd4d7b88b4011b3f615cf34acff62c
ms.sourcegitcommit: 47a002b7032a05ef67c4e5e12de7720062645e9e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2020
ms.locfileid: "81518192"
---
# <a name="base64_decode_toarray"></a>base64_decode_toarray()

base64 文字列を長い値の配列にデコードします。

**構文**

`base64_decode_toarray(`*文字列*`)`

**引数**

* *文字列*: base64 から UTF8-8 文字列にデコードされる入力文字列。

**戻り値**

base64 文字列から ecode を使用して長整数型の配列を返します。

* base64 文字列を UTF-8 文字列にデコードする場合は[、base64_decode_tostring() を](base64_decode_tostringfunction.md)参照してください。
* base64 文字列へのエンコード文字列については[、base64_encode_tostring() を](base64_encode_tostringfunction.md)参照してください。

**例**

```kusto
print Quine=base64_decode_toarray("S3VzdG8=")  
// 'K', 'u', 's', 't', 'o'
```

|クワイン|
|-----|
|[75,117,115,116,111]|

無効な UTF-8 エンコーディングから生成された base64 文字列をデコードしようとすると、null が返されます。

```kusto
print Empty=base64_decode_toarray("U3RyaW5n0KHR0tGA0L7Rh9C60LA=")
```

|Empty|
|-----|
||