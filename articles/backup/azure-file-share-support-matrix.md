---
title: Azure ファイル共有のバックアップのサポート マトリックス
description: Azure ファイル共有をバックアップする際のサポート設定と制限の概要について説明します。
ms.topic: conceptual
ms.date: 5/07/2020
ms.custom: references_regions
ms.openlocfilehash: 38c066c20399b39d676fb0c25aa158993258b979
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100370990"
---
# <a name="support-matrix-for-azure-file-share-backup"></a>Azure ファイル共有のバックアップのサポート マトリックス

[Azure Backup サービス](./backup-overview.md)を使用して、Azure ファイル共有をバックアップできます。 この記事では、Azure Backup を使用して Azure ファイル共有をバックアップするときのサポート設定について説明します。

> [!NOTE]
> Azure Backup では現在、NFS 共有はサポートされていません。

## <a name="supported-regions"></a>サポートされているリージョン

### <a name="ga-regions-for-azure-file-shares-backup"></a>Azure ファイル共有のバックアップの GA リージョン

Azure ファイル共有のバックアップは、以下を **除く** すべてのリージョンで利用できます。ドイツ中部 (ソブリン)、ドイツ北東部 (ソブリン)、中国東部、中国東部 2、中国北部、中国北部 2、US Gov アイオワ

## <a name="supported-storage-accounts"></a>サポートされるストレージ アカウント

| ストレージ アカウントの詳細 | サポート                                                      |
| ------------------------ | ------------------------------------------------------------ |
| アカウントの種類            | Azure Backup は、汎用 v1、汎用 v2、およびファイル ストレージの種類のストレージ アカウントに存在する Azure ファイル共有をサポートしています |
| パフォーマンス              | Azure Backup は、Standard および Premium Storage アカウントの両方でファイル共有をサポートしています |
| レプリケーション              | ストレージ アカウントでは、任意のレプリケーションの種類の Azure ファイル共有がサポートされています |
| ファイアウォールが有効         | Microsoft Azure サービスにストレージ アカウントへのアクセスを許可するファイアウォール規則があるストレージ アカウントでの Azure ファイル共有がサポートされています|

## <a name="supported-file-shares"></a>サポートされているファイル共有

| ファイル共有の種類                                   | サポート   |
| -------------------------------------------------- | --------- |
| Standard                                           | サポートされています |
| Large                                              | サポートされています |
| Premium                                            | サポートされています |
| Azure File 同期サービスに接続されているファイル共有 | サポートされています |

## <a name="protection-limits"></a>保護の制限

| 設定                                                      | 制限 |
| ------------------------------------------------------------ | ----- |
| コンテナーごとに保護できる 1 日あたりのファイル共有の最大数| 200   |
| コンテナーごとの 1 日あたりで登録できるストレージ アカウントの最大数 | 50    |
| コンテナーごとに保護できるファイル共有の最大数 | 2000   |
| コンテナーごとに登録できるストレージ アカウントの最大数 | 200   |

## <a name="backup-limits"></a>Backup の制限

| 設定                                      | 制限 |
| -------------------------------------------- | ----- |
| 1 日あたりのオンデマンド バックアップの最大数 | 10   |
| 1 日あたりのスケジュールされたバックアップの最大数 | 1     |

## <a name="restore-limits"></a>復元の制限

| 設定                                                      | 制限   |
| ------------------------------------------------------------ | ------- |
| 1 日あたりの復元の最大数                           | 10      |
| 復元あたりのファイルの最大数                         | 99      |
| 大きいファイル共有の場合に、復元あたりの推奨される最大復元サイズ | 15 TiB |

## <a name="retention-limits"></a>保有の制限

| 設定                                                      | 制限    |
| ------------------------------------------------------------ | -------- |
| 任意の時点でのファイル共有あたりの最大合計回復ポイント数 | 200      |
| オンデマンド バックアップによって作成される回復ポイントの最大保有期間 | 10 年間 |
| ファイル共有あたりの毎日の回復ポイント (スナップショット) の最大保有期間| 200 日間 |
| ファイル共有あたりの毎週の回復ポイント (スナップショット) の最大保有期間 | 200 週間 |
| ファイル共有あたりの毎月の回復ポイント (スナップショット) の最大保有期間 | 120 か月間 |
| ファイル共有あたりの毎年の回復ポイント (スナップショット) の最大保有期間 | 10 年間 |

## <a name="supported-restore-methods"></a>サポートされる復元方法

| 復元方法     | 詳細                                                      |
| ------------------ | ------------------------------------------------------------ |
| 完全な共有の復元 | ファイル共有全体を元の場所または別の場所に復元することができます。 |
| 項目レベルの復元 | 個々のファイルやフォルダーを元の場所または別の場所に復元することができます。 |

## <a name="next-steps"></a>次のステップ

* [Azure ファイル共有のバックアップ](backup-afs.md)方法について学習する
* [Azure ファイル共有を復元する](restore-afs.md)方法について確認する
* [Azure ファイル共有のバックアップを管理する](manage-afs-backup.md)方法について確認する
