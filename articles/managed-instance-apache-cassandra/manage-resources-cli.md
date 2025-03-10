---
title: Azure CLI を利用して Azure Managed Instance for Apache Cassandra リソースを管理する
description: Azure CLI を使用して Azure Managed Instance for Apache Cassandra の管理を自動化するための一般的なコマンドについて説明します。
author: TheovanKraay
ms.service: managed-instance-apache-cassandra
ms.topic: how-to
ms.date: 03/02/2021
ms.author: thvankra
ms.openlocfilehash: 3cd5fdbf6cdc504a1290c8fbd80cf89cf85ce714
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743857"
---
# <a name="manage-azure-managed-instance-for-apache-cassandra-resources-using-azure-cli-preview"></a>Azure CLI を利用して Azure Managed Instance for Apache Cassandra リソースを管理する (プレビュー)

この記事では、Azure CLI を使用して Azure Managed Instance for Apache Cassandra クラスターの管理を自動化するための一般的なコマンドについて説明します。

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra は現在、パブリック プレビューの段階にあります。
> このプレビュー バージョンはサービス レベル アグリーメントなしで提供されています。運用環境のワークロードに使用することはお勧めできません。 特定の機能はサポート対象ではなく、機能が制限されることがあります。
> 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

* この記事には、Azure CLI バージョン 2.12.1 以降が必要です。 Azure Cloud Shell を使用している場合は、最新バージョンが既にインストールされています。

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra リソースの名前を変更することはできません。Azure Resource Manager におけるリソース URI の取り扱いに違反するためです。

## <a name="azure-managed-instance-for-apache-cassandra-clusters"></a>Azure Managed Instance for Apache Cassandra クラスター

以降のセクションでは、Azure Managed Instance for Apache Cassandra クラスターを管理する方法について説明します。次のセクションがあります。

* [マネージド インスタンス クラスターを作成する](#create-cluster)
* [マネージド インスタンス クラスターを削除する](#delete-cluster)
* [クラスターの詳細を取得する](#get-cluster-details)
* [クラスター ノードの状態を取得する](#get-cluster-status)
* [リソース グループでクラスターを一覧表示する](#list-clusters-resource-group)
* [サブスクリプション ID でクラスターを一覧表示する](#list-clusters-subscription)

### <a name="create-a-managed-instance-cluster"></a><a id="create-cluster"></a>マネージド インスタンス クラスターを作成する

Azure Managed Instance for Apache Cassandra クラスターを作成します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
location='West US'
delegatedManagementSubnetId='/subscriptions/<subscription id>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/management'
initialCassandraAdminPassword='myPassword'

# You can override the cluster name if the original name is not legal for an Azure resource.
# overrideClusterName='ClusterNameIllegalForAzureResource'
# the default Cassandra version is v3.11

az managed-cassandra cluster create \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName \
    --location $location \
    --delegated-management-subnet-id $delegatedManagementSubnetId \
    --initial-cassandra-admin-password $initialCassandraAdminPassword \
```

### <a name="delete-a-managed-instance-cluster"></a><a id="delete-cluster"></a>マネージド インスタンス クラスターを削除する

クラスターを削除します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra cluster delete \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="get-the-cluster-details"></a><a id="get-cluster-details"></a>クラスターの詳細を取得する

クラスターの詳細を取得します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra cluster show \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="get-the-cluster-node-status"></a><a id="get-cluster-status"></a>クラスター ノードの状態を取得する

クラスターの詳細を取得します。

```azurecli-interactive
clusterName='cassandra-hybrid-cluster'
resourceGroupName='MyResourceGroup'

az managed-cassandra cluster node-status \
    --cluster-name $clusterName \
    --resource-group $resourceGroupName
```

### <a name="list-the-clusters-by-resource-group"></a><a id="list-clusters-resource-group"></a>リソース グループでクラスターを一覧表示する

リソース グループでクラスターを一覧表示します。

```azurecli-interactive
subscriptionId='MySubscriptionId'
resourceGroupName='MyResourceGroup'

az managed-cassandra cluster list\
    --resource-group $resourceGroupName
```

### <a name="list-clusters-by-subscription-id"></a><a id="list-clusters-subscription"></a>サブスクリプション ID でクラスターを一覧表示する

サブスクリプション ID でクラスターを一覧表示します。

```azurecli-interactive
# set your subscription id
az account set -s <subscription id>

az managed-cassandra cluster list
```

## <a name="the-managed-instance-datacenters"></a><a id="managed-instance-datacenter"></a>マネージド インスタンスのデータセンター

以降のセクションでは、Azure Managed Instance for Apache Cassandra のデータセンターを管理する方法について説明します。次のセクションがあります。

* [データセンターを作成する](#create-datacenter)
* [データセンターを削除する](#delete-datacenter)
* [データセンターの詳細を取得する](#get-datacenter-details)
* [データセンターを更新またはスケーリングする](#update-datacenter)
* [クラスター内のデータセンターを取得する](#get-datacenters-cluster)

### <a name="create-a-datacenter"></a><a id="create-datacenter"></a>データセンターを作成する

データセンターを作成します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'
dataCenterLocation='eastus2'
delegatedSubnetId='/subscriptions/<Subscription_ID>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/dc1-subnet'

az managed-cassandra datacenter create \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName \
    --data-center-location $dataCenterLocation \
    --delegated-subnet-id $delegatedSubnetId \
    --node-count 3 
```

### <a name="delete-a-datacenter"></a><a id="delete-datacenter"></a>データセンターを削除する

データセンターを削除します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'

az managed-cassandra datacenter delete \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName 
```

### <a name="get-datacenter-details"></a><a id="get-datacenter-details"></a>データセンターの詳細を取得する

データセンターの詳細を取得します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'

az managed-cassandra datacenter show \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName 
```

### <a name="update-or-scale-a-datacenter"></a><a id="update-datacenter"></a>データセンターを更新またはスケーリングする

データセンターを更新またはスケーリング (nodeCount 値をスケール変更) します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'
dataCenterName='dc1'
dataCenterLocation='eastus'
delegatedSubnetId= '/subscriptions/<Subscription_ID>/resourceGroups/customer-vnet-rg/providers/Microsoft.Network/virtualNetworks/customer-vnet/subnets/dc1-subnet'

az managed-cassandra datacenter update \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName \
    --data-center-name $dataCenterName \
    --node-count 13 
```

### <a name="get-the-datacenters-in-a-cluster"></a><a id="get-datacenters-cluster"></a>クラスター内のデータセンターを取得する

クラスター内のデータセンターを取得します。

```azurecli-interactive
resourceGroupName='MyResourceGroup'
clusterName='cassandra-hybrid-cluster'

az managed-cassandra datacenter list \
    --resource-group $resourceGroupName \
    --cluster-name $clusterName
```

## <a name="next-steps"></a>次のステップ

* [Azure portal からマネージド インスタンス クラスターを作成する](create-cluster-portal.md)
* [Azure Databricks でマネージド Apache Spark クラスターをデプロイする](deploy-cluster-databricks.md)