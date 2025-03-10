---
title: 管理アドレス
description: App Service Environment をコントロールするために使用される管理アドレスを見つけます。 それらは、非対称ルーティングの問題を回避するためにルート テーブル エントリで構成されます。
author: ccompy
ms.assetid: a7738a24-89ef-43d3-bff1-77f43d5a3952
ms.topic: article
ms.date: 02/11/2021
ms.author: ccompy
ms.custom: seodec18, references_regions
ms.openlocfilehash: 7810ca71ccdd8714773ffd459ef38b4bf80de02c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100371619"
---
# <a name="app-service-environment-management-addresses"></a>App Service Environment の管理アドレス

App Service Environment (ASE) は、ご使用の Azure Virtual Network (VNet) 内で実行する Azure App Service のシングル テナント デプロイです。  ASE は、VNet 内で実行されますが、サービス管理のために Azure App Service で使用される多数の専用 IP アドレスから依然としてアクセスできます。  ASE の場合、管理トラフィックは、ユーザーによって制御されるネットワークを横断します。 このトラフィックがブロックまたは誤ってルーティングされていると、ASE は中断されます。 ASE ネットワークの依存関係の詳細については、「[App Service Environment のネットワークの考慮事項][networking]」を参照してください。 ASE に関する一般情報については、まず「[Azure App Service Environment の概要][intro]」を参照してください。

すべての ASE に、管理トラフィックの送信先パブリック VIP があります。 こうしたアドレスからの着信管理トラフィックは、ASE のパブリック VIP のポート 454 および 455 に送信されます。 このドキュメントでは、ASE に対する管理トラフィックの APP Service ソース アドレスの一覧を示します。 これらのアドレスは、AppServiceManagement という名前の IP サービス タグ内にもあります。

次に示すアドレスをルート テーブル内で構成すると、管理トラフィックに関する非対称ルーティングの問題を回避できます。 ルートは、IP レベルでトラフィックに基づいて機能します。トラフィックの方向や、トラフィックが TCP 応答メッセージの一部であることは認識しません。 TCP 要求の応答アドレスが送信先アドレスと異なる場合は、非対称ルーティングの問題があります。 ASE 管理トラフィックに関する非対称ルーティングの問題を避けるには、応答が送信先アドレスと同じアドレスから返されることを確認します。 送信トラフィックがオンプレミスで送信される環境内で動作するように ASE を構成する方法の詳細については、[強制トンネリングを使用した ASE の構成][forcedtunnel]に関する記事を参照してください

## <a name="list-of-management-addresses"></a>管理アドレスの一覧 ##

| リージョン | アドレス |
|--------|-----------|
| すべてのパブリック リージョン | 13.66.140.0、13.67.8.128、13.69.64.128、13.69.227.128、13.70.73.128、13.71.170.64、13.71.194.129、13.75.34.192、13.75.127.117、13.77.50.128、13.78.109.0、13.89.171.0、13.94.141.115、13.94.143.126、13.94.149.179、20.36.106.128、20.36.114.64、23.102.135.246、23.102.188.65、40.69.106.128、40.70.146.128、40.71.13.64、40.74.100.64、40.78.194.128、40.79.130.64、40.79.178.128、40.83.120.64、40.83.121.56、40.83.125.161、40.112.242.192、51.140.146.64、51.140.210.128、52.151.25.45、52.162.106.192、52.165.152.214、52.165.153.122、52.165.154.193、52.165.158.140、52.174.22.21、52.178.177.147、52.178.184.149、52.178.190.65、52.178.195.197、52.187.56.50、52.187.59.251、52.187.63.19、52.187.63.37、52.224.105.172、52.225.177.153、52.231.18.64、52.231.146.128、65.52.172.237、70.37.57.58、104.44.129.141、104.44.129.243、104.44.129.255、104.44.134.255、104.208.54.11、104.211.81.64、104.211.146.128、157.55.208.185、191.233.203.64、191.236.154.88、52.181.183.11 |
| Microsoft Azure Government | 23.97.29.209、13.72.53.37、13.72.180.105、52.181.183.11、52.227.80.100、52.182.93.40、52.244.79.34、52.238.74.16 |
| Azure 中国 | 42.159.4.236、42.159.80.125 |

## <a name="configuring-a-network-security-group"></a>ネットワーク セキュリティ グループの構成

ネットワーク セキュリティ グループがあれば、個々のアドレスや、独自の構成を維持することについて心配する必要がありません。 すべてのアドレスを保持して最新の状態に保たれる AppServiceManagement という名前の IP サービス タグがあります。 この IP サービス タグを NSG で使用するには、ポータルに移動して [Network Security Groups UI]\(ネットワーク セキュリティ グループ UI\) を開き、[セキュリティ規則の受信] を選択します。 受信管理トラフィック用の既存の規則がある場合は、それを編集します。 この NSG がお使いの ASE で作成されたものでない場合や、すべて新しいものの場合は、 **[追加]** を選択します。 [ソース] ドロップダウンで、 **[Service Tag]\(サービス タグ)** を選択します。  [ソース サービス タグ] で **AppServiceManagement** を選択します。 ソース ポートの範囲を \*、[Destination]\(宛先) を **[Any]\(任意)** 、宛先ポート範囲を **[454 455]** 、プロトコルを **[TCP]** 、[アクション] を **[許可]** に設定します。 規則を作成する場合は、[優先度] を設定する必要があります。 

![サービス タグでの NSG の作成][1]

## <a name="configuring-a-route-table"></a>ルート テーブルの構成

すべての受信管理トラフィックが同じパスを戻ることができるように、管理アドレスは、インターネットの次ホップを指定してルート テーブル内に配置できます。 これらのルートは、強制トンネリングの構成時に必要です。 ルート テーブルを作成するには、ポータル、PowerShell、または Azure CLI を使用できます。  PowerShell プロンプトから Azure CLI を使用してルート テーブルを作成するコマンドは、次の通りです。 

```azurepowershell-interactive
$rg = "resource group name"
$rt = "route table name"
$location = "azure location"
$managementAddresses = "13.66.140.0", "13.67.8.128", "13.69.64.128", "13.69.227.128", "13.70.73.128", "13.71.170.64", "13.71.194.129", "13.75.34.192", "13.75.127.117", "13.77.50.128", "13.78.109.0", "13.89.171.0", "13.94.141.115", "13.94.143.126", "13.94.149.179", "20.36.106.128", "20.36.114.64", "23.102.135.246", "23.102.188.65", "40.69.106.128", "40.70.146.128", "40.71.13.64", "40.74.100.64", "40.78.194.128", "40.79.130.64", "40.79.178.128", "40.83.120.64", "40.83.121.56", "40.83.125.161", "40.112.242.192", "51.140.146.64", "51.140.210.128", "52.151.25.45", "52.162.106.192", "52.165.152.214", "52.165.153.122", "52.165.154.193", "52.165.158.140", "52.174.22.21", "52.178.177.147", "52.178.184.149", "52.178.190.65", "52.178.195.197", "52.187.56.50", "52.187.59.251", "52.187.63.19", "52.187.63.37", "52.224.105.172", "52.225.177.153", "52.231.18.64", "52.231.146.128", "65.52.172.237", "70.37.57.58", "104.44.129.141", "104.44.129.243", "104.44.129.255", "104.44.134.255", "104.208.54.11", "104.211.81.64", "104.211.146.128", "157.55.208.185", "191.233.203.64", "191.236.154.88", "52.181.183.11"

az network route-table create --name $rt --resource-group $rg --location $location
foreach ($ip in $managementAddresses) {
    az network route-table route create -g $rg --route-table-name $rt -n $ip --next-hop-type Internet --address-prefix ($ip + "/32")
}
```

ルート テーブルを作成した後は、お使いの ASE サブネットでそれを設定する必要があります。  

## <a name="get-your-management-addresses-from-api"></a>API から管理アドレスを取得する ##

次の API 呼び出しで、ご自身の ASE に一致する管理アドレスを一覧表示できます。

```http
get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01
```

API によって、ご自身の ASE に対するすべての受信アドレスが含まれる JSON ドキュメントが返されます。 アドレスの一覧には、管理アドレス、ご自身の ASE によって使用される VIP、および ASE サブネットのアドレス範囲自体が含まれています。  

[armclient](https://github.com/projectkudu/ARMClient) を使用して API を呼び出すには、次のコマンドを、サブスクリプション ID、リソース グループ、および ASE 名を置き換えて使用します。  

```azurepowershell-interactive
armclient login
armclient get /subscriptions/<subscription ID>/resourceGroups/<resource group>/providers/Microsoft.Web/hostingEnvironments/<ASE Name>/inboundnetworkdependenciesendpoints?api-version=2016-09-01
```

<!--IMAGES-->
[1]: ./media/management-addresses/managementaddr-nsg.png

<!-- LINKS -->
[networking]: ./network-info.md
[intro]: ./intro.md
[forcedtunnel]: ./forced-tunnel-support.md
