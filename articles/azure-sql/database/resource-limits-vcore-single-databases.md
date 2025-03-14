---
title: 単一データベースの仮想コア リソースの制限
description: このページでは、Azure SQL Database の単一データベースに対するいくつかの一般的な仮想コア リソース制限について説明します。
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: reference
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/22/2021
ms.openlocfilehash: 1fec13eefad7f27bcaac8f2c690b99909cd24e59
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100518047"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-purchasing-model"></a>仮想コア購入モデルを使用した単一データベースに対するリソース制限
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

この記事では、仮想コア購入モデルを使用した、Azure SQL Database の単一データベースに対する詳細なリソース制限について説明します。

サーバー上の単一データベースに対する DTU 購入モデルの制限については、[サーバー上のリソース制限の概要](resource-limits-logical-server.md)に関するページを参照してください。

[Azure portal](single-database-manage.md#the-azure-portal)、[Transact-SQL](single-database-manage.md#transact-sql-t-sql)、[PowerShell](single-database-manage.md#powershell)、[Azure CLI](single-database-manage.md#the-azure-cli)、または [REST API](single-database-manage.md#rest-api) を使って、単一のデータベースにサービス レベル、コンピューティング サイズ (サービス目標)、ストレージ容量を設定できます。

> [!IMPORTANT]
> スケーリングのガイダンスと考慮事項については、[単一データベースのスケーリング](single-database-scale.md)に関するページを参照してください。

## <a name="general-purpose---serverless-compute---gen5"></a>General Purpose - サーバーレス コンピューティング - Gen5

[サーバーレス コンピューティング レベル](serverless-tier-overview.md)は、現在 Gen5 ハードウェアでのみ使用できます。

### <a name="gen5-compute-generation-part-1"></a>Gen5 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:--- | --: |--: |--: |--: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|最小最大の仮想コア|0.5 から 1|0.5 から 2|0.5 から 4|0.75 から 6|1.0 から 8|
|最小最大のメモリ (GB)|2.02 から 3|2.05 から 6|2.10 から 12|2.25 から 18|3.00 から 24|
|最小最大の自動一時停止の遅延 (分)|60 から 10080|60 から 10080|60 から 10080|60 から 10080|60 から 10080|
|列ストアをサポート|はい*|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|
|データの最大サイズ (GB)|512|1024|1024|1024|1536|
|最大ログ サイズ (GB)|154|307|307|307|461|
|TempDB の最大データ サイズ (GB)|32|64|128|192|256|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS \*\*|320|640|1280|1920|2560|
|最大ログ速度 (Mbps)|4.5|9|18|27|36|
|最大同時実行ワーカー (要求) 数|75|150|該当なし|450|600|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* 最大仮想コア構成が小さいサービス目標では、列ストア インデックスを作成して使用するためのメモリが不足している可能性があります。  列ストアでパフォーマンスの問題が発生した場合は、最大仮想コア構成を大きくして、使用可能な最大メモリを増やしてください。  
\*\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="gen5-compute-generation-part-2"></a>Gen5 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:--- | --: |--: |--: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|最小最大の仮想コア|1.25 から 10|1.50 から 12|1.75 ～ 14|2.00 から 16|
|最小最大のメモリ (GB)|3.75 から 30|4.50 から 36|5.25 から 42|6.00 から 48|
|最小最大の自動一時停止の遅延 (分)|60 から 10080|60 から 10080|60 から 10080|60 から 10080|
|列ストアをサポート|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|N/A|該当なし|
|データの最大サイズ (GB)|1536|3072|3072|3072|
|最大ログ サイズ (GB)|461|461|461|922|
|TempDB の最大データ サイズ (GB)|320|384|448|512|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|3200|3840|4480|5120|
|最大ログ速度 (Mbps)|36|36|36|36|
|最大同時実行ワーカー (要求) 数|750|900|1050|1200|
|最大同時セッション数|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|
|マルチ AZ|該当なし|該当なし|N/A|該当なし|
|読み取りスケールアウト|該当なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="gen5-compute-generation-part-3"></a>Gen5 コンピューティング世代 (パート 3)

|コンピューティング サイズ (サービス目標)|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:--- | --: |--: |--: |--: |--:|
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|最小最大の仮想コア|2.25 から 18|2.5 から 20|3 ～ 24|4 から 32|5 から 40|
|最小最大のメモリ (GB)|6.75 から 54|7.5 から 60|9 から 72|12 から 96|15 から 120|
|最小最大の自動一時停止の遅延 (分)|60 から 10080|60 から 10080|60 から 10080|60 から 10080|60 から 10080|
|列ストアをサポート|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|N/A|該当なし|
|データの最大サイズ (GB)|3072|3072|4096|4096|4096|
|最大ログ サイズ (GB)|922|922|1024|1024|1024|
|TempDB の最大データ サイズ (GB)|576|640|768|1024|1280|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|5760|6400|7680|10240|12800|
|最大ログ速度 (Mbps)|36|36|36|36|36|
|最大同時実行ワーカー (要求) 数|1350|1500|1800|2400|3000|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|
|マルチ AZ|該当なし|該当なし|該当なし|N/A|該当なし|
|読み取りスケールアウト|該当なし|該当なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。


## <a name="hyperscale---provisioned-compute---gen4"></a>Hyperscale - プロビジョニング済みコンピューティング - Gen4

### <a name="gen4-compute-generation-part-1"></a>Gen4 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|HS_Gen4_1|HS_Gen4_2|HS_Gen4_3|HS_Gen4_4|HS_Gen4_5|HS_Gen4_6|
|:--- | --: |--: |--: |---: | --: |--: |
|コンピューティング世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|仮想コア|1|2|3|4|5|6|
|メモリ (GB)|7|14|21|28|35|42|
|[RBPEX](service-tier-hyperscale.md#compute) サイズ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |100 |100|
|最大ログ サイズ (TB)|無制限 |無制限 |無制限 |無制限 |無制限 |無制限 |
|TempDB の最大データ サイズ (GB)|32|64|96|128|160|192|
|ストレージの種類| [注 1](#notes) |[注 1](#notes)|[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |
|最大ローカル SSD IOPS *|4000 |8000 |12000 |16000 |20000 |24000 |
|最大ログ速度 (Mbps)|100 |100 |100 |100 |100 |100 |
|IO 待機時間 (概算)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1000|1200|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|セカンダリ レプリカ|0-4|0-4|0-4|0-4|0-4|0-4|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|バックアップ ストレージのリテンション期間|7 日|7 日|7 日|7 日|7 日|7 日|
|||

\* ローカル SSD IO に加えて、ワークロードでは、リモート [ページ サーバー](service-tier-hyperscale.md#page-server) IO が使用されます。 有効な IOPS はワークロードによって異なります。 詳細については、[データ IO ガバナンス](resource-limits-logical-server.md#resource-governance)に関する記事、および「[リソース使用率の統計でのデータ IO](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)」を参照してください。

### <a name="gen4-compute-generation-part-2"></a>Gen4 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|HS_Gen4_7|HS_Gen4_8|HS_Gen4_9|HS_Gen4_10|HS_Gen4_16|HS_Gen4_24|
|:--- | ---: |--: |--: | --: |--: |--: |
|コンピューティング世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|仮想コア|7|8|9|10|16|24|
|メモリ (GB)|49|56|63|70|112|159.5|
|[RBPEX](service-tier-hyperscale.md#compute) サイズ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |100 |100 |
|最大ログ サイズ (TB)|無制限 |無制限 |無制限 |無制限 |無制限 |無制限 |
|TempDB の最大データ サイズ (GB)|224|256|288|320|512|768|
|ストレージの種類| [注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |
|最大ローカル SSD IOPS *|28000 |32000 |36000 |40000 |64000 |76800 |
|最大ログ速度 (Mbps)|100 |100 |100 |100 |100 |100 |
|IO 待機時間 (概算)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|
|最大同時実行ワーカー (要求) 数|1400|1600|1800|2000|3200|4800|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|セカンダリ レプリカ|0-4|0-4|0-4|0-4|0-4|0-4|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|バックアップ ストレージのリテンション期間|7 日|7 日|7 日|7 日|7 日|7 日|
|||

\* ローカル SSD IO に加えて、ワークロードでは、リモート [ページ サーバー](service-tier-hyperscale.md#page-server) IO が使用されます。 有効な IOPS はワークロードによって異なります。 詳細については、[データ IO ガバナンス](resource-limits-logical-server.md#resource-governance)に関する記事、および「[リソース使用率の統計でのデータ IO](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)」を参照してください。

## <a name="hyperscale---provisioned-compute---gen5"></a>Hyperscale - プロビジョニング済みコンピューティング - Gen5

### <a name="gen5-compute-generation-part-1"></a>Gen5 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|HS_Gen5_2|HS_Gen5_4|HS_Gen5_6|HS_Gen_8|HS_Gen5_10|HS_Gen5_12|HS_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|仮想コア|2|4|6|8|10|12|14|
|メモリ (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|[RBPEX](service-tier-hyperscale.md#compute) サイズ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |100 |100 |100|
|最大ログ サイズ (TB)|無制限 |無制限 |無制限 |無制限 |無制限 |無制限 |無制限 |
|TempDB の最大データ サイズ (GB)|64|128|192|256|320|384|448|
|ストレージの種類| [注 1](#notes) |[注 1](#notes)|[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |
|最大ローカル SSD IOPS *|8000 |16000 |24000 |32000 |40000 |48000 |56000 |
|最大ログ速度 (Mbps)|100 |100 |100 |100 |100 |100 |100 |
|IO 待機時間 (概算)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1000|1200|1400|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|セカンダリ レプリカ|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|はい|
|バックアップ ストレージのリテンション期間|7 日|7 日|7 日|7 日|7 日|7 日|7 日|
|||

\* ローカル SSD IO に加えて、ワークロードでは、リモート [ページ サーバー](service-tier-hyperscale.md#page-server) IO が使用されます。 有効な IOPS はワークロードによって異なります。 詳細については、[データ IO ガバナンス](resource-limits-logical-server.md#resource-governance)に関する記事、および「[リソース使用率の統計でのデータ IO](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)」を参照してください。

### <a name="gen5-compute-generation-part-2"></a>Gen5 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|HS_Gen5_16|HS_Gen5_18|HS_Gen5_20|HS_Gen_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: |--: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|仮想コア|16|18|20|24|32|40|80|
|メモリ (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|[RBPEX](service-tier-hyperscale.md#compute) サイズ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |100 |100 |100 |
|最大ログ サイズ (TB)|無制限 |無制限 |無制限 |無制限 |無制限 |無制限 |無制限 |
|TempDB の最大データ サイズ (GB)|512|576|640|768|1024|1280|2560|
|ストレージの種類| [注 1](#notes) |[注 1](#notes)|[注 1](#notes)|[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |[注 1](#notes) |
|最大ローカル SSD IOPS *|64000 |72000 |80000 |96000 |128000 |160000 |204800 |
|最大ログ速度 (Mbps)|100 |100 |100 |100 |100 |100 |100 |
|IO 待機時間 (概算)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|
|最大同時実行ワーカー (要求) 数|1600|1800|2000|2400|3200|4000|8000|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|セカンダリ レプリカ|0-4|0-4|0-4|0-4|0-4|0-4|0-4|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|N/A|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|はい|
|バックアップ ストレージのリテンション期間|7 日|7 日|7 日|7 日|7 日|7 日|7 日|
|||

\* ローカル SSD IO に加えて、ワークロードでは、リモート [ページ サーバー](service-tier-hyperscale.md#page-server) IO が使用されます。 有効な IOPS はワークロードによって異なります。 詳細については、[データ IO ガバナンス](resource-limits-logical-server.md#resource-governance)に関する記事、および「[リソース使用率の統計でのデータ IO](hyperscale-performance-diagnostics.md#data-io-in-resource-utilization-statistics)」を参照してください。

#### <a name="notes"></a>Notes

**注 1**: Hyperscale は、個別のコンピューティングとストレージ コンポーネントを含む多層アーキテクチャです。[Hyperscale サービス レベルのアーキテクチャ](service-tier-hyperscale.md#distributed-functions-architecture)

**注 2**: ローカル コンピューティング レプリカ SSD 上にあるデータの待機時間は 1 から 2 ミリ秒です。これにより、最も使用されているデータ ページがキャッシュされます。 ページ サーバーから取得したデータは、待機時間が長くなります。

## <a name="hyperscale---provisioned-compute---dc-series"></a>Hyperscale - プロビジョニング済みコンピューティング - DC シリーズ

|コンピューティング サイズ (サービス目標)|HS_DC_2|HS_DC_4|HS_DC_6|HS_DC_8|
|:--- | --: |--: |--: |--: |---: | 
|コンピューティング世代|DC シリーズ|DC シリーズ|DC シリーズ|DC シリーズ|
|仮想コア|2|4|6|8|
|メモリ (GB)|9|18|27|36|
|[RBPEX](service-tier-hyperscale.md#compute) サイズ|3X メモリ|3X メモリ|3X メモリ|3X メモリ|
|列ストアをサポート|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|なし|該当なし|N/A|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |
|最大ログ サイズ (TB)|無制限 |無制限 |無制限 |無制限 |
|TempDB の最大データ サイズ (GB)|64|128|192|256|
|ストレージの種類| [注 1](#notes) |[注 1](#notes)|[注 1](#notes) |[注 1](#notes) |
|最大ローカル SSD IOPS *|8000 |16000 |24000 |32000 |
|最大ログ速度 (Mbps)|100 |100 |100 |100 |
|IO 待機時間 (概算)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|[注 2](#notes)|
|最大同時実行ワーカー (要求) 数|160|320|480|640|
|最大同時セッション数|30,000|30,000|30,000|30,000|
|セカンダリ レプリカ|0-4|0-4|0-4|0-4|
|マルチ AZ|なし|なし|N/A|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|
|バックアップ ストレージのリテンション期間|7 日|7 日|7 日|7 日|
|||

### <a name="notes"></a>Notes

**注 1**: Hyperscale は、個別のコンピューティングとストレージ コンポーネントを含む多層アーキテクチャです。[Hyperscale サービス レベルのアーキテクチャ](service-tier-hyperscale.md#distributed-functions-architecture)

**注 2**: ローカル コンピューティング レプリカ SSD 上にあるデータの待機時間は 1 から 2 ミリ秒です。これにより、最も使用されているデータ ページがキャッシュされます。 ページ サーバーから取得したデータは、待機時間が長くなります。

## <a name="general-purpose---provisioned-compute---gen4"></a>General Purpose - プロビジョニング済みコンピューティング - Gen4

> [!IMPORTANT]
> 新しい Gen4 データベースは、オーストラリア東部とブラジル南部リージョンでサポートされなくなりました。

### <a name="gen4-compute-generation-part-1"></a>Gen4 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|コンピューティング世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|仮想コア|1|2|3|4|5|6|
|メモリ (GB)|7|14|21|28|35|42|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|なし|なし|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|1024|1024|1536|1536|1536|3072|
|最大ログ サイズ (GB)|307|307|461|461|461|922|
|TempDB の最大データ サイズ (GB)|32|64|96|128|160|192|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|320|640|960|1280|1600|1920|
|最大ログ速度 (Mbps)|4.5|9|13.5|18|22.5|27|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1000|1200|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|1|
|マルチ AZ|なし|なし|なし|なし|N/A|該当なし|
|読み取りスケールアウト|なし|なし|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="gen4-compute-generation-part-2"></a>Gen4 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|コンピューティング世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|仮想コア|7|8|9|10|16|24|
|メモリ (GB)|49|56|63|70|112|159.5|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|なし|なし|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|3072|3072|3072|3072|4096|4096|
|最大ログ サイズ (GB)|922|922|922|922|1229|1229|
|TempDB の最大データ サイズ (GB)|224|256|288|320|512|768|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)
|最大データ IOPS *|2240|2560|2880|3200|5120|7680|
|最大ログ速度 (Mbps)|31.5|36|36|36|36|36|
|最大同時実行ワーカー (要求) 数|1400|1600|1800|2000|3200|4800|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|1|
|マルチ AZ|なし|なし|なし|なし|N/A|該当なし|
|読み取りスケールアウト|なし|なし|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

## <a name="general-purpose---provisioned-compute---gen5"></a>General Purpose - プロビジョニング済みコンピューティング - Gen5

### <a name="gen5-compute-generation-part-1"></a>Gen5 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|仮想コア|2|4|6|8|10|12|14|
|メモリ (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|1024|1024|1536|1536|1536|3072|3072|
|最大ログ サイズ (GB)|307|307|461|461|461|922|922|
|TempDB の最大データ サイズ (GB)|64|128|192|256|320|384|384|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|640|1280|1920|2560|3200|3840|4480|
|最大ログ速度 (Mbps)|9|18|27|36|36|36|36|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1000|1200|1400|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|1|1|
|マルチ AZ|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|
|読み取りスケールアウト|なし|なし|なし|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="gen5-compute-generation-part-2"></a>Gen5 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|仮想コア|16|18|20|24|32|40|80|
|メモリ (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|3072|3072|3072|4096|4096|4096|4096|
|最大ログ サイズ (GB)|922|922|922|1024|1024|1024|1024|
|TempDB の最大データ サイズ (GB)|512|576|640|768|1024|1280|2560|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|5120|5760|6400|7680|10240|12800|12800|
|最大ログ速度 (Mbps)|36|36|36|36|36|36|36|
|最大同時実行ワーカー (要求) 数|1600|1800|2000|2400|3200|4000|8000|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|1|1|
|マルチ AZ|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[プレビューで利用可能](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|
|読み取りスケールアウト|なし|なし|なし|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>General Purpose - プロビジョニング済みコンピューティング - Fsv2 シリーズ

### <a name="fsv2-series-compute-generation-part-1"></a>Fsv2 シリーズのコンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14| GP_Fsv2_16|
|:---| ---:|---:|---:|---:|---:|
|コンピューティング世代|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|
|仮想コア|8|10|12|14|16|
|メモリ (GB)|15.1|18.9|22.7|26.5|30.2|
|列ストアをサポート|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|なし|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|1024|1024|1024|1024|1536|
|最大ログ サイズ (GB)|336|336|336|336|512|
|TempDB の最大データ サイズ (GB)|333|333|333|333|333|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|2560|3200|3840|4480|5120|
|最大ログ速度 (Mbps)|36|36|36|36|36|
|最大同時実行ワーカー (要求) 数|400|500|600|700|800|
|最大同時ログイン数|800|1000|1200|1400|1600|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|
|マルチ AZ|なし|なし|なし|N/A|該当なし|
|読み取りスケールアウト|なし|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="fsv2-series-compute-generation-part-2"></a>Fsv2 シリーズのコンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|GP_Fsv2_18|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32| GP_Fsv2_36|GP_Fsv2_72|
|:---| ---:|---:|---:|---:|---:|---:|
|コンピューティング世代|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|Fsv2 シリーズ|
|仮想コア|18|20|24|32|36|72|
|メモリ (GB)|34.0|37.8|45.4|60.5|68.0|136.0|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|なし|なし|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|1536|1536|1536|3072|3072|4096|
|最大ログ サイズ (GB)|512|512|512|1024|1024|1024|
|TempDB の最大データ サイズ (GB)|83.25|92.5|111|148|166.5|333|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|5760|6400|7680|10240|11520|12800|
|最大ログ速度 (Mbps)|36|36|36|36|36|36|
|最大同時実行ワーカー (要求) 数|900|1000|1200|1600|1800|3600|
|最大同時ログイン数|1800|2000|2400|3200|3600|7200|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|1|1|
|マルチ AZ|なし|なし|なし|なし|N/A|該当なし|
|読み取りスケールアウト|なし|なし|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

## <a name="general-purpose---provisioned-compute---dc-series"></a>汎用 - プロビジョニング済みコンピューティング - DC シリーズ

|コンピューティング サイズ (サービス目標)|GP_DC_2|GP_DC_4|GP_DC_6|GP_DC_8| 
|:---| ---:|---:|---:|---:|
|コンピューティング世代|DC シリーズ|DC シリーズ|DC シリーズ|DC シリーズ|
|仮想コア|2|4|6|8|
|メモリ (GB)|9|18|27|36|
|列ストアをサポート|はい|○|はい|はい|
|インメモリ OLTP ストレージ (GB)|なし|なし|N/A|該当なし|
|データの最大サイズ (GB)|1024|1536|3072|3072|
|最大ログ サイズ (GB)|307|461|922|922|
|TempDB の最大データ サイズ (GB)|64|128|192|256|
|ストレージの種類|リモート SSD|リモート SSD|リモート SSD|リモート SSD|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|最大データ IOPS *|640|1280|1920|2560|
|最大ログ速度 (Mbps)|9|18|27|36|
|最大同時実行ワーカー (要求) 数|160|320|480|640|
|最大同時セッション数|30,000|30,000|30,000|30,000|
|レプリカの数|1|1|1|1|
|マルチ AZ|なし|なし|N/A|該当なし|
|読み取りスケールアウト|なし|該当なし|N/A|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|


\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

## <a name="business-critical---provisioned-compute---gen4"></a>Business Critical - プロビジョニング済みコンピューティング - Gen4

> [!IMPORTANT]
> 新しい Gen4 データベースは、オーストラリア東部とブラジル南部リージョンでサポートされなくなりました。

### <a name="gen4-compute-generation-part-1"></a>Gen4 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|コンピューティング世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|仮想コア|1|2|3|4|5|6|
|メモリ (GB)|7|14|21|28|35|42|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|1|2|3|4|5|6|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|データの最大サイズ (GB)|1024|1024|1024|1024|1024|1024|
|最大ログ サイズ (GB)|307|307|307|307|307|307|
|TempDB の最大データ サイズ (GB)|32|64|96|128|160|192|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS *|4,000|8,000|12,000|16,000|20,000|24,000|
|最大ログ速度 (Mbps)|8|16|24|32|40|48|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1000|1200|
|最大同時ログイン数|200|400|600|800|1000|1200|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|4|4|4|4|4|4|
|マルチ AZ|○|○|○|○|はい|はい|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="gen4-compute-generation-part-2"></a>Gen4 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|コンピューティング世代|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|仮想コア|7|8|9|10|16|24|
|メモリ (GB)|49|56|63|70|112|159.5|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|7|8|9.5|11|20|36|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|データの最大サイズ (GB)|1024|1024|1024|1024|1024|1024|
|最大ログ サイズ (GB)|307|307|307|307|307|307|
|TempDB の最大データ サイズ (GB)|224|256|288|320|512|768|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS |28,000|32,000|36,000|40,000|64,000|76,800|
|最大ログ速度 (Mbps)|56|64|64|64|64|64|
|最大同時実行ワーカー (要求) 数|1400|1600|1800|2000|3200|4800|
|最大同時ログイン (要求) 数|1400|1600|1800|2000|3200|4800|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|4|4|4|4|4|4|
|マルチ AZ|はい|はい|はい|はい|はい|はい|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

## <a name="business-critical---provisioned-compute---gen5"></a>Business Critical - プロビジョニング済みコンピューティング - Gen5

### <a name="gen5-compute-generation-part-1"></a>Gen5 コンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|仮想コア|2|4|6|8|10|12|14|
|メモリ (GB)|10.4|20.8|31.1|41.5|51.9|62.3|72.7|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|1.57|3.14|4.71|6.28|8.65|11.02|13.39|
|データの最大サイズ (GB)|1024|1024|1536|1536|1536|3072|3072|
|最大ログ サイズ (GB)|307|307|461|461|461|922|922|
|TempDB の最大データ サイズ (GB)|64|128|192|256|320|384|448|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS *|8000|16,000|24,000|32,000|40,000|48,000|56,000|
|最大ログ速度 (Mbps)|24|48|72|96|96|96|96|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1000|1200|1400|
|最大同時ログイン数|200|400|600|800|1000|1200|1400|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|4|4|4|4|4|4|4|
|マルチ AZ|はい|はい|はい|はい|はい|はい|はい|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

### <a name="gen5-compute-generation-part-2"></a>Gen5 コンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|コンピューティング世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|第 5 世代|
|仮想コア|16|18|20|24|32|40|80|
|メモリ (GB)|83|93.4|103.8|124.6|166.1|207.6|415.2|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|15.77|18.14|20.51|25.25|37.94|52.23|131.64|
|データの最大サイズ (GB)|3072|3072|3072|4096|4096|4096|4096|
|最大ログ サイズ (GB)|922|922|922|1024|1024|1024|1024|
|TempDB の最大データ サイズ (GB)|512|576|640|768|1024|1280|2560|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS *|64,000|72,000|80,000|96,000|128,000|160,000|204,800|
|最大ログ速度 (Mbps)|96|96|96|96|96|96|96|
|最大同時実行ワーカー (要求) 数|1600|1800|2000|2400|3200|4000|8000|
|最大同時ログイン数|1600|1800|2000|2400|3200|4000|8000|
|最大同時セッション数|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|レプリカの数|4|4|4|4|4|4|4|
|マルチ AZ|はい|はい|はい|はい|はい|はい|はい|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

## <a name="business-critical---provisioned-compute---m-series"></a>Business Critical - プロビジョニング済みコンピューティング - M シリーズ

### <a name="m-series-compute-generation-part-1"></a>M シリーズのコンピューティング世代 (パート 1)

|コンピューティング サイズ (サービス目標)|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:---| ---:|---:|---:|---:|---:|---:|
|コンピューティング世代|M シリーズ|M シリーズ|M シリーズ|M シリーズ|M シリーズ|M シリーズ|
|仮想コア|8|10|12|14|16|18|
|メモリ (GB)|235.4|294.3|353.2|412.0|470.9|529.7|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|64|80|96|112|128|150|
|データの最大サイズ (GB)|512|640|768|896|1024|1152|
|最大ログ サイズ (GB)|171|213|256|299|341|384|
|TempDB の最大データ サイズ (GB)|256|320|384|448|512|576|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS *|12,499|15,624|18,748|21,873|24,998|28,123|
|最大ログ速度 (Mbps)|48|60|72|84|96|108|
|最大同時実行ワーカー (要求) 数|800|1,000|1,200|1,400|1,600|1,800|
|最大同時ログイン数|800|1,000|1,200|1,400|1,600|1,800|
|最大同時セッション数|30000|30000|30000|30000|30000|30000|
|レプリカの数|4|4|4|4|4|4|
|マルチ AZ|いいえ|いいえ|いいえ|いいえ|いいえ|いいえ|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

> [!IMPORTANT]
> 場合によっては、未使用領域を再利用できるようにデータベースを縮小する必要があります。 詳細については、「[Manage file space in Azure SQL Database](file-space-manage.md)」(Azure SQL Database でファイル領域を管理する) を参照してください。

### <a name="m-series-compute-generation-part-2"></a>M シリーズのコンピューティング世代 (パート 2)

|コンピューティング サイズ (サービス目標)|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:---| ---:|---:|---:|---:|---:|
|コンピューティング世代|M シリーズ|M シリーズ|M シリーズ|M シリーズ|M シリーズ|
|仮想コア|20|24|32|64|128|
|メモリ (GB)|588.6|706.3|941.8|1883.5|3767.0|
|列ストアをサポート|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|172|216|304|704|1768|
|データの最大サイズ (GB)|1280|1536|2048|4096|4096|
|最大ログ サイズ (GB)|427|512|683|1024|1024|
|TempDB の最大データ サイズ (GB)|4096|2048|1024|768|640|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS *|31,248|37,497|49,996|99,993|160,000|
|最大ログ速度 (Mbps)|120|144|192|264|264|
|最大同時実行ワーカー (要求) 数|2,000|2,400|3,200|6,400|12,800|
|最大同時ログイン数|2,000|2,400|3,200|6,400|12,800|
|最大同時セッション数|30000|30000|30000|30000|30000|
|レプリカの数|4|4|4|4|4|
|マルチ AZ|いいえ|いいえ|いいえ|いいえ|いいえ|
|読み取りスケールアウト|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。

> [!IMPORTANT]
> 場合によっては、未使用領域を再利用できるようにデータベースを縮小する必要があります。 詳細については、「[Manage file space in Azure SQL Database](file-space-manage.md)」(Azure SQL Database でファイル領域を管理する) を参照してください。

## <a name="business-critical---provisioned-compute---dc-series"></a>Business Critical - プロビジョニング済みコンピューティング - DC シリーズ

|コンピューティング サイズ (サービス目標)|BC_DC_2|BC_DC_4|BC_DC_6|BC_DC_8|
|:--- | --: |--: |--: |--: |
|コンピューティング世代|DC シリーズ|DC シリーズ|DC シリーズ|DC シリーズ|
|仮想コア|2|4|6|8|
|メモリ (GB)|9|18|27|36|
|列ストアをサポート|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|1.7|3.7|5.9|8.2|
|データの最大サイズ (GB)|768|768|768|768|
|最大ログ サイズ (GB)|230|230|230|230|
|TempDB の最大データ サイズ (GB)|64|128|192|256|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|最大データ IOPS *|14000|28000|42000|56000|
|最大ログ速度 (Mbps)|24|48|72|96|
|最大同時実行ワーカー (要求) 数|200|400|600|800|
|最大同時ログイン数|200|400|600|800|
|最大同時セッション数|30,000|30,000|30,000|30,000|
|レプリカの数|4|4|4|4|
|マルチ AZ|いいえ|いいえ|いいえ|いいえ|
|読み取りスケールアウト|いいえ|いいえ|いいえ|いいえ|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

\* IO サイズの最大値。範囲は 8 KB ～ 64 KB。 実際の IOPS はワークロードに依存します。 詳細については、[データ IO のガバナンス](resource-limits-logical-server.md#resource-governance)に関するページを参照してください。


## <a name="next-steps"></a>次のステップ

- 単一データベースに対する DTU のリソース制限については、[DTU 購入モデルを使用した単一データベースに対するリソース制限](resource-limits-dtu-single-databases.md)に関するページを参照してください
- エラスティック プールに対する仮想コア リソースの制限については、[仮想コア購入モデルを使用したエラスティック プールに対するリソース制限](resource-limits-vcore-elastic-pools.md)に関するページを参照してください
- エラスティック プールに対する DTU リソースの制限については、「[DTU ベースの購入モデルを使用したエラスティック プールのリソース制限](resource-limits-dtu-elastic-pools.md)」を参照してください
- SQL Managed Instance に対するリソース制限については、[SQL Managed Instance のリソース制限](../managed-instance/resource-limits.md)に関するページを参照してください。
- Azure の一般的な制限については、「[Azure サブスクリプションとサービスの制限、クォータ、制約](../../azure-resource-manager/management/azure-subscription-service-limits.md)」をご覧ください。
- サーバー上でのリソース制限については、サーバーおよびサブスクリプション レベルの制限に関する情報が記載された、[サーバー上のリソース制限の概要](resource-limits-logical-server.md)に関するページを参照してください。
