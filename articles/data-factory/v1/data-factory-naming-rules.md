---
title: Azure Data Factory エンティティの名前付け規則 - バージョン 1
description: Data Factory v1 エンティティの名前付け規則について説明します。
author: dcstwh
ms.author: weetok
ms.reviewer: maghan
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 83621a7ceeae32ea4b55e3f22fff61d50e8cdb60
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100380170"
---
# <a name="rules-for-naming-azure-data-factory-entities"></a>Azure Data Factory エンティティの名前付け規則
> [!NOTE]
> この記事は、Data Factory のバージョン 1 に適用されます。 現在のバージョンの Data Factory サービスを使用している場合は、[Data Factory の名前付け規則](../naming-rules.md)に関するページを参照してください。

次の表に、Data Factory アーティファクトの名前付け規則を示します。

| Name | 名前の一意性 | 検証チェック |
|:--- |:--- |:--- |
| Data Factory |Microsoft Azure 全体で一意です。 名前の大文字と小文字を区別されません。つまり、`MyDF` と `mydf` は同じデータ ファクトリを表します。 |<ul><li>各データ ファクトリは、厳密に 1 つの Azure サブスクリプションに関連付けられます。</li><li>オブジェクト名は英文字または数字で始まり、英文字、数字、ダッシュ (-) 文字のみを含めることができます。</li><li>すべてのダッシュ (-) 文字は、その直前または直後に文字または数字が使用されている必要があります。 連続するダッシュ文字はコンテナー名では使用できません。</li><li>名前は 3 ～ 63 文字の長さにすることができます。</li></ul> |
| リンクされたサービス/テーブル/パイプライン |データ ファクトリ内で一意です。 名前の大文字と小文字は区別されません。 |<ul><li>テーブル名の最大文字数: 260。</li><li>オブジェクト名は、文字、数字、アンダー スコア (_) のいずれかで始める必要があります。</li><li>次の文字は使用できません: "."、"+"、"?"、"/"、"<"、">"、"*"、"%"、"&"、":"、"\\"</li></ul> |
| リソース グループ |Microsoft Azure 全体で一意です。 名前の大文字と小文字は区別されません。 |<ul><li>最大文字数: 1,000。</li><li>名前には、文字、数字、および次の文字を含めることができます: "-"、"_"、","、"."</li></ul> |

