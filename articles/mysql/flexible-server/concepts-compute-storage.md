---
title: コンピューティングとストレージのオプション - Azure Database for MySQL - フレキシブル サーバー
description: この記事では、Azure Database for MySQL - フレキシブル サーバーのコンピューティングとストレージのオプションについて説明します。
author: Bashar-MSFT
ms.author: bahusse
ms.service: mysql
ms.topic: conceptual
ms.date: 1/28/2021
ms.openlocfilehash: 9b5a2c5d004e63c602a30f7808586e97a0e436e8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101720938"
---
# <a name="compute-and-storage-options-in-azure-database-for-mysql---flexible-server-preview"></a>Azure Database for MySQL のコンピューティングとストレージのオプション - フレキシブル サーバー (プレビュー)

> [!IMPORTANT] 
> Azure Database for MySQL - フレキシブル サーバーは現在、パブリック プレビュー段階にあります。

Azure Database for MySQL フレキシブル サーバーは、次の 3 つのコンピューティング レベルのいずれかで作成できます。バースト可能、汎用、メモリ最適化。 コンピューティング レベルは、B シリーズ、D シリーズ、および E シリーズで使用される、基になる VM SKU によって区別されます。 選択したコンピューティング レベルおよびサイズで、サーバーで使用できるメモリと仮想コアが決まります。 同じストレージ テクノロジが、すべてのコンピューティング レベルで使用されます。 リソースはすべて、MySQL サーバー レベルでプロビジョニングされます。 1 つのサーバーには 1 つ以上のデータベースを含めることができます。

| リソース/レベル | **バースト可能** | **汎用** | **メモリ最適化** |
|:---|:----------|:--------------------|:---------------------|
| VM シリーズ| B シリーズ | Ddsv4 シリーズ | Edsv4 シリーズ|
| 仮想コア | 1、2 | 2、4、8、16、32、48、64 | 2、4、8、16、32、48、64 |
| 仮想コアあたりのメモリ | 変数 | 4 GiB | 8 GiB* |
| ストレージ サイズ | 5 GiB ～ 16 TiB | 5 GiB ～ 16 TiB | 5 GiB ～ 16 TiB |
| データベース バックアップのリテンション期間 | 1 ～ 35 日間 | 1 ～ 35 日間 | 1 ～ 35 日間 |

\* 504 GB のメモリを備えた E64ds_v4 (メモリ最適化) SKU を除く

コンピューティング レベルを選択するには、最初に次の表を使用してください。

| コンピューティング レベル | 対象のワークロード |
|:-------------|:-----------------|
| バースト可能 | 最大の CPU を継続的に必要とするわけではないワークロードに最適です。 |
| General Purpose | 負荷分散されたコンピューティングとメモリ、およびスケーラブルな I/O スループットを必要とする大部分のビジネス ワークロード。 たとえば、Web アプリやモバイル アプリ、その他のエンタープライズ アプリケーションをホストするためのサーバーが挙げられます。|
| メモリ最適化 | 高速トランザクション処理と高いコンカレンシーを実現するためのインメモリ パフォーマンスを必要とする、高パフォーマンス データベース ワークロード。 たとえば、リアルタイム データと高パフォーマンスなトランザクション アプリや分析アプリを処理するためのサーバーが挙げられます。|

サーバーを作成した後に、コンピューティング レベル、コンピューティング サイズ、およびストレージ サイズが変更されました。 コンピューティングのスケーリングには再起動が必要で、これに 60 ～ 120 秒かかるのに対し、ストレージのスケーリングには再起動は不要です。 また、他の調整とは関係なく、バックアップの保持期間の長さを調整することもできます。 詳細については、「[リソースのスケール](#scale-resources)」セクションを参照してください。

## <a name="compute-tiers-size-and-server-types"></a>コンピューティング レベル、サイズ、およびサーバーの種類

コンピューティング リソースは、そのレベルとサイズに基づいて選択することができます。 これにより、仮想コアとメモリ サイズが決まります。 仮想コアは、基になるハードウェアの論理 CPU を表します。

使用可能なサーバーの種類の詳細な仕様は次のとおりです。

| コンピューティング サイズ         | 仮想コア | メモリ サイズ (GiB) | 
|----------------------|--------|-------------------|
| **バースト可能**        |        |                   | 
| Standard_B1s         | 1      | 1                 |  
| Standard_B1ms        | 1      | 2                 | 
| Standard_B2s         | 2      | 4                 |  
| **汎用**  |        |                   | 
| Standard_D2ds_v4     | 2      | 8                 |  
| Standard_D4ds_v4     | 4      | 16                | 
| Standard_D8ds_v4     | 8      | 32                | 
| Standard_D16ds_v4    | 16     | 64                | 
| Standard_D32ds_v4    | 32     | 128               |  
| Standard_D48ds_v4    | 48     | 192               |  
| Standard_D64ds_v4    | 64     | 256               | 
| **メモリ最適化** |        |                   |
| Standard_E2ds_v4     | 2      | 16                |
| Standard_E4ds_v4     | 4      | 32                |
| Standard_E8ds_v4     | 8      | 64                |
| Standard_E16ds_v4    | 16     | 128               |
| Standard_E32ds_v4    | 32     | 256               |
| Standard_E48ds_v4    | 48     | 384               |
| Standard_E64ds_v4    | 64     | 504               |

使用可能なコンピューティング シリーズの詳細については、 [バースト可能 (B シリーズ)](../../virtual-machines/sizes-b-series-burstable.md)、[汎用 (Ddsv4 シリーズ)](../../virtual-machines/ddv4-ddsv4-series.md)、[メモリ最適化 (Edsv4 シリーズ)](../../virtual-machines/edv4-edsv4-series.md) に関する Azure VM ドキュメントを参照して、ください。

>[!NOTE]
>[バースト可能 (B シリーズ) コンピューティング レベル](../../virtual-machines/sizes-b-series-burstable.md)では、VM が開始/停止または再起動すると、クレジットが失われる可能性があります。 詳細については、[バースト可能 (B シリーズ) の FAQ](../../virtual-machines/sizes-b-series-burstable.md#q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart) に関する記事を参照してください。

## <a name="storage"></a>ストレージ

プロビジョニングするストレージは、フレキシブル サーバーで使用可能なストレージ容量です。 ストレージは、データベース ファイル、一時ファイル、トランザクション ログ、および MySQL サーバー ログに使用されます。 すべてのコンピューティング レベルで、サポートされる最小ストレージは 5 GiB で、最大ストレージは 16 TiB です。 ストレージは 1 GiB 単位でスケーリングされ、サーバーの作成後にスケールアップすることができます。

>[!NOTE]
> ストレージはスケールアップのみ可能で、スケールダウンすることはできません。

ストレージの上限、ストレージの割合、およびストレージで使用されるメトリックを使用して Azure portal、(Azure Monitor を含む) でストレージの使用量を監視できます。 メトリックの詳細については、 [監視に関する記事](./concepts-monitoring.md) を参照してください。 

### <a name="reaching-the-storage-limit"></a>容量の上限に到達

サーバーで使用されているストレージが、プロビジョニングされた制限に近づくと、サーバーが読み取り専用モードになって、サーバー上で失われた書き込みがすべて保護されます。 プロビジョニングされたストレージが 100 GiB 以下のサーバーでは、空きストレージが、プロビジョニングされたストレージ サイズの 5% 未満になると、読み取り専用とマークされます。 プロビジョニングされたストレージが 100 GiB を超えるサーバーは、空きストレージが 5 GiB 未満になると、読み取り専用とマークされます。

たとえば、110 GiB のストレージをプロビジョニングしている場合は、実際の使用量が 105 GiB を超えると、サーバーが読み取り専用とマークされます。 または、5 GiB のストレージをプロビジョニングしている場合は、ストレージの空き容量が 256 MB 未満になると、サーバーが読み取り専用としてマークされます。

サービスがサーバーを読み取り専用にしようとしている間、すべての新しい書き込みトランザクション要求はブロックされ、既存のアクティブなトランザクションの実行は継続されます。 サーバーが読み取り専用に設定された場合、後続のすべての書き込み操作とトランザクションのコミットは失敗します。 読み取りクエリは、中断せずに作業を続行します。 

サーバーの読み取り専用モードを解除するには、サーバーで、プロビジョニングされたストレージを増やす必要があります。 これを行うには、Azure portal または Azure CLI を使用します。 プロビジョニングされたストレージを増やすと、サーバーは再び、書き込みトランザクションが可能な状態になります。

サーバーのストレージがしきい値に近づいたときに、それを通知するアラートを設定しておくことで、読み取り専用状態に入るのを防ぐことをお勧めします。 使用可能なメトリックの詳細については、 [監視に関する記事](./concepts-monitoring.md) を参照してください。 

推奨される対策は、 <!--turn on storage auto-grow or to--> 読み取り専用状態になるのを防ぐために、サーバーのストレージがしきい値に近づいたときに通知するアラートを設定しておくことです。 詳細については、[アラートの設定方法](how-to-alert-on-metric.md)に関するドキュメントを参照してください。

### <a name="storage-auto-grow"></a>ストレージの自動拡張

ストレージの自動拡張は、Azure Database for MySQL フレキシブル サーバーでは、まだ使用できません。

## <a name="iops"></a>IOPS

Azure Database for MySQL – フレキシブル サーバーでは、追加の IOPS のプロビジョニングがサポートされます。 この機能を使用すると、無償の IOPS 制限を超えて追加の IOPS をプロビジョニングできます。 この機能を使用すると、ワークロードの要件に基づいてプロビジョニングされる IOPS の数をいつでも増減できます。 

最小 IOPS はすべてのコンピューティング サイズで 100 であり、最大 IOPS は選択したコンピューティング サイズによって決まります。 プレビューでは、サポートされる最大 IOPS は 20,000 IOPS です。

コンピューティング サイズごとの最大 IOPS の詳細については、以下を参照してください。 

| コンピューティング サイズ         | 最大 IOPS        | 
|----------------------|---------------------|
| **バースト可能**        |                     |
| Standard_B1s         | 320                 |
| Standard_B1ms        | 640                 |
| Standard_B2s         | 1280                | 
| **汎用**  |                     |
| Standard_D2ds_v4     | 3200                |
| Standard_D4ds_v4     | 6400                |
| Standard_D8ds_v4     | 12800               |
| Standard_D16ds_v4    | 20000               |
| Standard_D32ds_v4    | 20000               |
| Standard_D48ds_v4    | 20000               | 
| Standard_D64ds_v4    | 20000               | 
| **メモリ最適化** |                     | 
| Standard_E2ds_v4     | 3200                | 
| Standard_E4ds_v4     | 6400                | 
| Standard_ E8ds_v4    | 12800               | 
| Standard_ E16ds_v4   | 20000               | 
| Standard_E32ds_v4    | 20000               | 
| Standard_E48ds_v4    | 20000               | 
| Standard_E64ds_v4    | 20000               |  

最大 IOPS は、コンピューティング サイズごとの使用可能な最大 IOPS によって決まります。 列 *[キャッシュが無効な場合の最大ディスク スループット:IOPS/MBps]* を、[B シリーズ](../../virtual-machines/sizes-b-series-burstable.md)、[Ddsv4 シリーズ](../../virtual-machines/ddv4-ddsv4-series.md)、および [Edsv4 シリーズ](../../virtual-machines/edv4-edsv4-series.md)のドキュメントで調べてください。

> [!Important]
> **無償の IOPS** は、コンピューティング サイズの "キャッシュが無効な場合の最大ディスク スループット: IOPS/MBps"、GiB * 3 でプロビジョニングされたストレージのうちの小さい方と同等になります。<br>
> **最小 IOPS** は、すべてのコンピューティング サイズで 100 です。<br>
> **最大 IOPS** は、選択したコンピューティング サイズによって決まります。 プレビューでは、サポートされる最大 IOPS は 20,000 IOPS です。

Azure portal (Azure Monitor を含む) での自分の I/O 使用量は、[IO の割合](./concepts-monitoring.md)メトリクスを使用して監視できます。 コンピューティングに基づき、より多くの IOPS が必要になり、その後、最大 IOPS が必要になった場合は、サーバーのコンピューティングをスケーリングする必要があります。

## <a name="backup"></a>バックアップ

サービスによって、サーバーのバックアップが自動的に取得されます。 1 ～ 35 日間の範囲で保持期間を選択できます。 バックアップの詳細については、[バックアップと復元の概念に関する記事](concepts-backup-restore.md)を参照してください。

## <a name="scale-resources"></a>リソースのスケール

サーバーを作成した後は、コンピューティング レベル、コンピューティング サイズ (仮想コアおよびメモリ)、ストレージ容量、およびバックアップの保持期間を個別に変更できます。 コンピューティング サイズは、スケールアップまたはスケールダウンすることができます。 バックアップの保持期間は、1 ～ 35 日間の範囲でスケールアップまたはスケールダウンできます。 ストレージ サイズは増やすことのみ可能です。 リソースのスケーリングは、ポータルまたは Azure CLI を使用して実行できます。

> [!NOTE]
> ストレージ サイズは増やすことのみ可能です。 増加後に、より小さなストレージ サイズに戻すことはできません。

コンピューティング レベルまたはコンピューティング サイズを変更すると、サーバーが再起動され、新しいサーバーの種類が有効になります。 システムが新しいサーバーに切り替わるほんの短時間、新しい接続を確立できず、コミットされていないすべてのトランザクションがロールバックされます。 この時間の長さは変動しますが、ほとんどの場合は 60 ～ 120 秒です。 

ストレージのスケーリングおよびバックアップの保持期間の変更は、オンライン操作であり、サーバーの再起動は不要です。

## <a name="pricing"></a>価格

最新の料金情報については、サービスの[料金ページ](https://azure.microsoft.com/pricing/details/MySQL/)を参照してください。 必要な構成のコストについては、[Azure portal](https://portal.azure.com/#create/Microsoft.MySQLServer/flexibleServers) で、選択したオプションに基づいて **[コンピューティング + ストレージ]** タブに表示される月額コストをご覧ください。 Azure サブスクリプションを取得していない場合は、Azure 料金計算ツールを使用して見積もり価格を確認できます。 [Azure 料金計算ツール](https://azure.microsoft.com/pricing/calculator/)の Web サイトで **[項目の追加]** を選択し、 **[データベース]** カテゴリを展開します。 **[Azure Database for MySQL]** を選択し、デプロイの種類として **[フレキシブル サーバー]** を選択してオプションをカスタマイズします。

サーバーのコストを最適化する場合は、次のヒントを検討してください。

- コンピューティングの使用率が低すぎる場合は、コンピューティング レベルまたはコンピューティング サイズ (仮想コア) をスケールダウンします。
- ワークロードで、完全な計算能力が継続的には必要でない場合は、コンピューティング レベルを 「汎用」および「メモリ最適化」から「バースト可能」に切り替えることを検討してください。
- 使用されていない場合は、サーバーを停止します。
- バックアップを長期間、保有する必要がない場合は、バックアップの保持期間を短縮します。

## <a name="next-steps"></a>次のステップ

- [ポータルで MySQL サーバーを作成](quickstart-create-server-portal.md)する方法を確認します。
- [サービスの制限事項](concepts-limitations.md)を確認します。