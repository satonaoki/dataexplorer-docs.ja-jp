---
title: parse_path ()-Azure データエクスプローラー
description: この記事では、Azure データエクスプローラーの parse_path () について説明します。
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: reference
ms.date: 10/23/2018
ms.openlocfilehash: e2914e913402de7442d2533cf5159c2bd30fac60
ms.sourcegitcommit: 09da3f26b4235368297b8b9b604d4282228a443c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/28/2020
ms.locfileid: "87346305"
---
# <a name="parse_path"></a>parse_path()

ファイルパスを解析 `string` し、 [`dynamic`](./scalar-data-types/dynamic.md) パスの次の部分を含むオブジェクトを返します。
* Scheme
* RootPath
* DirectoryPath
* DirectoryName
* FileName
* 拡張機能
* AlternateDataStreamName

この関数では、両方の種類のスラッシュを持つ単純なパスに加えて、次のようなパスがサポートされています。
* スキーマ。 たとえば、"file://..." のようになります。
* 共有パス。 たとえば、"shareddrive\users..." のようになります。 \\
* 長いパス。 たとえば、" \\ ? \c:..." のようになります。
* 代替データ ストリーム。 たとえば、"file1.exe:file2.exe" のようになります。

## <a name="syntax"></a>構文

`parse_path(`*path*`)`

## <a name="arguments"></a>引数

* *パス*: ファイルパスを表す文字列。

## <a name="returns"></a>戻り値

`dynamic`上に示したパスコンポーネントを含む型のオブジェクト。

## <a name="example"></a>例

<!-- csl: https://help.kusto.windows.net/Samples -->
```kusto
datatable(p:string) 
[
    @"C:\temp\file.txt",
    @"temp\file.txt",
    "file://C:/temp/file.txt:some.exe",
    @"\\shared\users\temp\file.txt.gz",
    "/usr/lib/temp/file.txt"
]
| extend path_parts = parse_path(p)

```

|p|path_parts
|---|---
|C:\temp\file.txt|{"Scheme": "", "RootPath": "C:", "DirectoryPath": "C: \\ temp", "DirectoryName": "temp", "Filename": "file.txt", "Extension": "txt", "AlternateDataStreamName": ""}
|temp\file.txt|{"Scheme": "", "RootPath": "", "DirectoryPath": "temp", "DirectoryName": "temp", "Filename": "file.txt", "Extension": "txt", "AlternateDataStreamName": ""}
|file://C:/temp/file.txt:some.exe|{"Scheme": "file"、"RootPath": "C:"、"DirectoryPath": "C:/temp"、"DirectoryName": "temp"、"Filename": "file.txt"、"Extension": "txt"、"AlternateDataStreamName": "some.exe"}
|\\shared\users\temp\file.txt gz|{"Scheme": "", "RootPath": "", "DirectoryPath": " \\ \\shared \\ users \\ temp "," DirectoryName ":" temp "," Filename ":" file.txt. gz "," Extension ":" gz "," AlternateDataStreamName ":" "}
|/usr/lib/temp/file.txt|{"Scheme": "", "RootPath": "", "DirectoryPath": "/usr/lib/temp", "DirectoryName": "temp", "Filename": "file.txt", "Extension": "txt", "AlternateDataStreamName": ""}
