---
title: Azure Monitor CLI のサンプル
description: Azure Monitor の機能用のサンプル CLI コマンドです。 Azure Monitor は、アラート通知の送信、構成済みのテレメトリ データの値に基づく Web URL の呼び出しに加え、Cloud Services、Virtual Machines、Web Apps の自動スケーリングを行うことができる Microsoft Azure サービスです。
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/16/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: 8eaf8c2e140f0b323db0c20a2e9946884c51df04
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/04/2021
ms.locfileid: "102039174"
---
# <a name="azure-monitor-cli-samples"></a>Azure Monitor CLI のサンプル
この記事では、Azure Monitor の機能にアクセスするために役立つコマンド ライン インターフェイス (CLI) のサンプル コマンドを紹介します。 Azure Monitor では、Cloud Services、Virtual Machines、Web Apps を自動スケールできます。また、アラート通知の送信や、構成済みのテレメトリ データの値に基づく Web URL の呼び出しも行うことができます。

## <a name="prerequisites"></a>前提条件

Azure CLI をまだインストールしていない場合は、「[Azure CLI のインストール](/cli/azure/install-azure-cli)」の手順に従います。 [Azure Cloud Shell](/azure/cloud-shell) を使用して、ブラウザーの対話型機能として CLI を実行することもできます。 使用可能なすべてのコマンドについては、[Azure Monitor CLI リファレンス](/cli/azure/monitor)をご覧ください。 

## <a name="log-in-to-azure"></a>Azure にログインする
まず、Azure アカウントにログインします。

```azurecli
az login
```

このコマンドを実行した後、画面の指示に従ってサインインする必要があります。 すべてのコマンドは、既定のサブスクリプションのコンテキストで動作します。

現在のサブスクリプションの詳細を一覧表示します。

```azurecli
az account show
```

作業コンテキストを別のサブスクリプションに変更します。

```azurecli
az account set -s <Subscription ID or name>
```

サポートされているすべての Azure Monitor コマンドの一覧を表示します。

```azurecli
az monitor -h
```

## <a name="view-activity-log"></a>アクティビティ ログを表示する

監査ログ イベントの一覧を表示します。

```azurecli
az monitor activity-log list
```

使用可能なすべてのオプションを表示します。

```azurecli
az monitor activity-log list -h
```

resourceGroup を指定してログの一覧を表示します。

```azurecli
az monitor activity-log list --resource-group <group name>
```

呼び出し元を指定してログの一覧を表示します。

```azurecli
az monitor activity-log list --caller myname@company.com
```

日付の範囲、リソースの種類、および呼び出し元を指定して、ログの一覧を表示します。

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>アラートの操作 
> [!NOTE]
> 現時点で CLI でサポートされるのはアラート (クラシック) のみです。 

### <a name="get-alert-classic-rules-in-a-resource-group"></a>リソース グループのアラート (クラシック) の取得

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-classic-rule"></a>メトリック アラート (クラシック) の作成

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-classic-rule"></a>アラート (クラシック) ルールの削除

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>ログ プロファイル

このセクションの情報を使用すると、ログ プロファイルを操作できます。

### <a name="get-a-log-profile"></a>ログ プロファイルの取得

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>保有期間を指定したログ プロファイルの追加

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>保有期間とイベント ハブを指定したログ プロファイルの追加

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>ログ プロファイルの削除

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>診断

このセクションの情報を使用すると、診断設定を操作できます。

### <a name="get-a-diagnostic-setting"></a>診断設定の取得

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-setting"></a>診断設定の作成 

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <storage account ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

### <a name="delete-a-diagnostic-setting"></a>診断設定の削除

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>自動スケール

このセクションの情報を使用すると、自動スケール設定を操作できます。 これらの例を変更する必要があります。

### <a name="get-autoscale-settings-for-a-resource-group"></a>リソース グループの自動スケール設定の取得

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>名前によるリソース グループの自動スケール設定の取得

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>自動スケール設定の設定

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```
