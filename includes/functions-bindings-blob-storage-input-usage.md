---
title: インクルード ファイル
description: インクルード ファイル
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 938f55ae0ba911ea3a97cd49e6424bf8aaefdc76
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100381692"
---
### <a name="default"></a>Default

BLOB の入力バインドには、次のパラメーターの型を使用できます。

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup>*function.json* には "inout" バインド `direction`、C# クラス ライブラリには `FileAccess.ReadWrite` が必要です。

Storage SDK タイプの 1 つにバインドしようとしてエラー メッセージが表示された場合は、[適切な Storage SDK バージョン](../articles/azure-functions/functions-bindings-storage-blob.md#azure-storage-sdk-version-in-functions-1x)への参照があることをご確認ください。

`string` または `Byte[]` へのバインドが推奨されるのは、BLOB のサイズが小さい場合のみです (BLOB 全体のコンテンツがメモリに読み込まれるため)。 通常、`Stream` 型または `CloudBlockBlob` 型の使用が推奨されます。 詳しくは、この記事で前述した「[コンカレンシーとメモリ使用量](../articles/azure-functions/functions-bindings-storage-blob-trigger.md#concurrency-and-memory-usage)」セクションをご覧ください。

### <a name="additional-types"></a>その他の型

[Storage 拡張機能の 5.0.0 またはそれ以降のバージョン](../articles/azure-functions/functions-bindings-storage-blob.md#storage-extension-5x-and-higher)を使用するアプリでは、[Azure SDK for .NET](/dotnet/api/overview/azure/storage.blobs-readme) の型を使用することもできます。 このバージョンでは、次の型を優先して、レガシ `CloudBlobContainer`、`CloudBlobDirectory`、`ICloudBlob`、`CloudBlockBlob`、`CloudPageBlob`、`CloudAppendBlob` 型のサポートがなくなります。

- [BlobContainerClient](/dotnet/api/azure.storage.blobs.blobcontainerclient)
- [BlobClient](/dotnet/api/azure.storage.blobs.blobclient)<sup>1</sup>
- [BlockBlobClient](/dotnet/api/azure.storage.blobs.specialized.blockblobclient)<sup>1</sup>
- [PageBlobClient](/dotnet/api/azure.storage.blobs.specialized.pageblobclient)<sup>1</sup>
- [AppendBlobClient](/dotnet/api/azure.storage.blobs.specialized.appendblobclient)<sup>1</sup>
- [BlobBaseClient](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient)<sup>1</sup>

<sup>1</sup>*function.json* には "inout" バインド `direction`、C# クラス ライブラリには `FileAccess.ReadWrite` が必要です。

これらの型の使用例については、[拡張機能の GitHub リポジトリ](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/storage/Microsoft.Azure.WebJobs.Extensions.Storage.Blobs#examples)に関するページを参照してください。
