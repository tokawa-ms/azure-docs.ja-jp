---
title: Azure Firewall を使用してプライベート エンドポイント宛てのトラフィックを検査する
titleSuffix: Azure Private Link
description: Azure Firewall を使用して、プライベート エンドポイント宛てのトラフィックを検査する方法について説明します。
services: private-link
author: jocortems
ms.service: private-link
ms.topic: how-to
ms.date: 09/02/2020
ms.author: allensu
ms.openlocfilehash: 3ed349616ae6456913c19bb073f6e9ea28e7d549
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "100575126"
---
# <a name="use-azure-firewall-to-inspect-traffic-destined-to-a-private-endpoint"></a>Azure Firewall を使用してプライベート エンドポイント宛てのトラフィックを検査する

Azure プライベート エンドポイントは、Azure Private Link の基本的な構成要素です。 プライベート エンドポイントを使用すると、仮想ネットワークにデプロイされた Azure リソースから、プライベート リンク リソースとプライベートに通信できます。

プライベート エンドポイントを使用すると、リソースから、仮想ネットワークにデプロイされたプライベート リンク サービスにアクセスできます。 仮想ネットワーク ピアリングとオンプレミス ネットワーク接続を介したプライベート エンドポイントへのアクセスにより、接続性が拡張されます。

場合によっては、プライベート エンドポイントを介して公開されるサービスに対するクライアントからのトラフィックを検査またはブロックする必要があります。 [Azure Firewall](../firewall/overview.md) またはサード パーティのネットワーク仮想アプライアンスを使用して、この検査を完了します。

次の制限事項が適用されます。

* ネットワーク セキュリティ グループ (NSG) はプライベート エンドポイントには適用されません
* ユーザー定義ルート (UDR) はプライベート エンドポイントには適用されません
* サブネットにアタッチできるルート テーブルは 1 つです
* ルート テーブルでは最大 400 ルートがサポートされます

Azure Firewall では、次のいずれかを使用してトラフィックをフィルター処理します。

* TCP および UDP プロトコルの場合、[ネットワーク ルール内の FQDN](../firewall/fqdn-filtering-network-rules.md)
* HTTP、HTTPS、MSSQL の場合、[アプリケーション ルール内の FQDN](../firewall/features.md#application-fqdn-filtering-rules) 

プライベート エンドポイント経由で公開されるほとんどのサービスでは、HTTPS を使用します。 Azure SQL を使用する場合は、ネットワーク ルールよりもアプリケーション ルールを使用することをお勧めします。

> [!NOTE]
> SQL の FQDN のフィルター処理は、[プロキシ モード](../azure-sql/database/connectivity-architecture.md#connection-policy)のみでサポートされます (ポート 1433)。 **プロキシ** モードを使用すると、*リダイレクト* と比較して待機時間が増加する可能性があります。 リダイレクト モードを使用して構成する場合は (Azure 内から接続するクライアントの既定)、代わりにファイアウォール ネットワーク ルールで FQDN を使ってアクセスをフィルター処理できます。

## <a name="scenario-1-hub-and-spoke-architecture---dedicated-virtual-network-for-private-endpoints"></a>シナリオ 1:ハブとスポークのアーキテクチャ - プライベート エンドポイント専用の仮想ネットワーク

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/hub-and-spoke.png" alt-text="プライベート エンドポイント専用の仮想ネットワーク" border="true":::

このシナリオは、プライベート エンドポイントを使用して複数の Azure サービスにプライベートに接続するための、最も拡張性のあるアーキテクチャです。 プライベート エンドポイントがデプロイされるネットワーク アドレス空間を指すルートが作成されます。 この構成により、管理オーバーヘッドが軽減され、400 ルートの上限に達するのを防ぐことができます。

仮想ネットワークがピアリングされている場合、クライアント仮想ネットワークからハブ仮想ネットワーク内の Azure Firewall に接続すると料金が発生します。

ピアリングされた仮想ネットワークとの接続に関連した料金の詳細については、[価格](https://azure.microsoft.com/pricing/details/private-link/)ページの FAQ セクションを参照してください。

>[!NOTE]
> このシナリオは、アプリケーション ルールではなく、サード パーティの NVA または Azure Firewall ネットワーク ルールを使用して実装できます。

## <a name="scenario-2-hub-and-spoke-architecture---shared-virtual-network-for-private-endpoints-and-virtual-machines"></a>シナリオ 2: ハブとスポークのアーキテクチャ - プライベート エンドポイントと仮想マシン用の共有仮想ネットワーク

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/shared-spoke.png" alt-text="同じ仮想ネットワーク内のプライベート エンドポイントと仮想マシン" border="true":::

このシナリオは、次の場合に実装されます。

* プライベート エンドポイント専用の仮想ネットワークを用意できない

* プライベート エンドポイントを使用して仮想ネットワークで公開されるサービスの数が少ない

仮想マシンには、個々のプライベート エンドポイントを指す /32 システム ルートがあります。 Azure Firewall 経由でトラフィックをルーティングするために、プライベート エンドポイントごとに 1 つのルート構成されます。 

仮想ネットワークでサービスが公開されると、ルート テーブル保守の管理オーバーヘッドが増加します。 ルート制限に達する可能性も高くなります。

全体のアーキテクチャによっては、400 ルートの上限に達する可能性があります。 可能な限り、シナリオ 1 を使用することをお勧めします。

仮想ネットワークがピアリングされている場合、クライアント仮想ネットワークからハブ仮想ネットワーク内の Azure Firewall に接続すると料金が発生します。

ピアリングされた仮想ネットワークとの接続に関連した料金の詳細については、[価格](https://azure.microsoft.com/pricing/details/private-link/)ページの FAQ セクションを参照してください。

>[!NOTE]
> このシナリオは、アプリケーション ルールではなく、サード パーティの NVA または Azure Firewall ネットワーク ルールを使用して実装できます。

## <a name="scenario-3-single-virtual-network"></a>シナリオ 3: 単一の仮想ネットワーク

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/single-vnet.png" alt-text="単一の仮想ネットワーク" border="true":::

実装にはいくつかの制限があります。ハブとスポークのアーキテクチャへの移行はできません。 シナリオ 2 と同じ考慮事項が適用されます。 このシナリオでは、仮想ネットワークのピアリング料金は適用されません。

>[!NOTE]
> サード パーティの NVA または Azure Firewall を使用してこのシナリオを実装する場合、SNAT トラフィックをプライベート エンドポイント宛てにするには、アプリケーション ルールではなくネットワーク ルールを使用する必要があります。 そうしないと、仮想マシンとプライベート エンドポイント間の通信は失敗します。

## <a name="scenario-4-on-premises-traffic-to-private-endpoints"></a>シナリオ 4: プライベート エンドポイントへのオンプレミス トラフィック

:::image type="content" source="./media/inspect-traffic-using-azure-firewall/on-premises.png" alt-text="プライベート エンドポイントへのオンプレミス トラフィック" border="true":::

このアーキテクチャは、次のいずれかを使用してオンプレミス ネットワークとの接続を構成した場合に実装できます。 

* [ExpressRoute](..\expressroute\expressroute-introduction.md)
* [サイト間 VPN](../vpn-gateway/tutorial-site-to-site-portal.md) 

セキュリティの要件により、プライベート エンドポイントを介して公開されるサービスへのクライアント トラフィックをセキュリティ アプライアンス経由でルーティングする必要がある場合は、このシナリオをデプロイします。

前出のシナリオ 2 と同じ考慮事項が適用されます。 このシナリオでは、仮想ネットワークのピアリング料金はありません。 オンプレミスのワークロードがプライベート エンドポイントにアクセスできるように DNS サーバーを構成する方法の詳細については、「[DNS フォワーダーを使用しているオンプレミスのワークロード](./private-endpoint-dns.md#on-premises-workloads-using-a-dns-forwarder)」を参照してください。

>[!NOTE]
> サード パーティの NVA または Azure Firewall を使用してこのシナリオを実装する場合、SNAT トラフィックをプライベート エンドポイント宛てにするには、アプリケーション ルールではなくネットワーク ルールを使用する必要があります。 そうしないと、仮想マシンとプライベート エンドポイント間の通信は失敗します。

## <a name="prerequisites"></a>前提条件

* Azure サブスクリプション。
* Log Analytics ワークスペース。  

サブスクリプションにワークスペースがない場合に作成する方法については、「[Azure portal で Log Analytics ワークスペースを作成する](../azure-monitor/logs/quick-create-workspace.md)」を参照してください。


## <a name="sign-in-to-azure"></a>Azure へのサインイン

Azure Portal ( https://portal.azure.com ) にサインインします。

## <a name="create-a-vm"></a>VM の作成

このセクションでは、プライベート リンク リソースへのアクセスに使用する VM をホストするために、仮想ネットワークとサブネットを作成します。 サービスの例として Azure SQL データベースを使用します。

## <a name="virtual-networks-and-parameters"></a>仮想ネットワークとパラメーター

次の目的のために、3 つの仮想ネットワークと、それぞれに対応するサブネットを作成します。

* VM とプライベート エンドポイント間の通信を制限するために使用される Azure Firewall を含めます。
* プライベート リンク リソースにアクセスするために使用される VM をホストします。
* プライベート エンドポイントをホストします。

手順の中で、各パラメーターを次のように置き換えます。

### <a name="azure-firewall-network"></a>Azure Firewall ネットワーク
| パラメーター                   | 値                 |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | myAzFwVNet          |
| **\<region-name>**          | 米国中南部      |
| **\<IPv4-address-space>**   | 10.0.0.0/16          |
| **\<subnet-name>**          | AzureFirewallSubnet        |
| **\<subnet-address-range>** | 10.0.0.0/24          |

### <a name="virtual-machine-network"></a>仮想マシンのネットワーク
| パラメーター                   | 値                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | myVMVNet          |
| **\<region-name>**          | 米国中南部      |
| **\<IPv4-address-space>**   | 10.1.0.0/16          |
| **\<subnet-name>**          | VMSubnet      |
| **\<subnet-address-range>** | 10.1.0.0/24          |

### <a name="private-endpoint-network"></a>プライベート エンドポイント ネットワーク
| パラメーター                   | 値                 |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroup |
| **\<virtual-network-name>** | myPEVNet         |
| **\<region-name>**          | 米国中南部      |
| **\<IPv4-address-space>**   | 10.2.0.0/16          |
| **\<subnet-name>**          | PrivateEndpointSubnet    |        |
| **\<subnet-address-range>** | 10.2.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

10. 手順 1 ～ 9 を繰り返して、仮想マシンとプライベート エンドポイント リソースをホストするための仮想ネットワークを作成します。

### <a name="create-virtual-machine"></a>仮想マシンの作成

1. Azure portal の画面の左上で、 **[リソースの作成]**  >  **[Compute]**  >  **[仮想マシン]** を選択します。

2. **[仮想マシンの作成 - 基本]** に次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | **プロジェクトの詳細** | |
    | サブスクリプション | サブスクリプションを選択します。 |
    | Resource group | **[myResourceGroup]** を選択します。 このリソース グループは、前のセクションで作成しました。  |
    | **インスタンスの詳細** |  |
    | 仮想マシン名 | 「**myVM**」と入力します。 |
    | リージョン | **[(米国) 米国中南部]** を選択します。 |
    | 可用性のオプション | 既定値 **[インフラストラクチャ冗長は必要ありません]** をそのまま使用します。 |
    | Image | **[Ubuntu Server 18.04 LTS - Gen1]** を選択します。 |
    | サイズ | **[Standard_B2s]** を選択します。 |
    | **管理者アカウント** |  |
    | 認証の種類 | **[パスワード]** を選択します。 |
    | ユーザー名 | 任意のユーザー名を入力します。 |
    | Password | 任意のパスワードを入力します。 パスワードは 12 文字以上で、[定義された複雑さの要件](../virtual-machines/linux/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm)を満たす必要があります。|
    | パスワードの確認 | パスワードを再入力します。 |
    | **受信ポートの規則** |  |
    | パブリック受信ポート | **[なし]** を選択します。 |
    |||

3. **ディスク** を選択します。

4. **[仮想マシンの作成 - Disk]** で、既定値のままにし、 **[Next: Networking]\(次へ : ネットワーク\)** を選択します。

5. **[仮想マシンの作成 - ネットワーク]** で次の情報を選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | 仮想ネットワーク | **[myVMVNet]** を選択します。  |
    | Subnet | **[VMSubnet (10.1.0.0/24)]** を選択します。|
    | パブリック IP | 既定値 **(new) myVm-ip\((新規) myVm-ip** のままにします。 |
    | パブリック受信ポート | **[選択したポートを許可する]** を選択します。 |
    | 受信ポートの選択 | **[SSH]** を選択します。|
    ||

6. **[Review + create]\(レビュー + 作成\)** を選択します。 **[確認および作成]** ページが表示され、Azure によって構成が検証されます。

7. "**証に成功しました**" というメッセージが表示されたら、 **[作成]** を選択します。

## <a name="deploy-the-firewall"></a>ファイアウォールのデプロイ

1. Azure portal メニュー上または **[ホーム]** ページから **[リソースの作成]** を選択します。

2. 検索ボックスに「**ファイアウォール**」と入力し、**Enter** キーを押します。

3. **[ファイアウォール]** を選択し、 **[作成]** を選択します。

4. **[ファイアウォールの作成]** ページで、次の表を使用してファイアウォールを構成します。

    | 設定 | 値 |
    | ------- | ----- |
    | **プロジェクトの詳細** | |
    | サブスクリプション | サブスクリプションを選択します。 |
    | Resource group | **[myResourceGroup]** を選択します。  |
    | **インスタンスの詳細** |  |
    | 名前 | 「**myAzureFirewall**」と入力します。 |
    | リージョン | **[米国中南部]** を選択します。 |
    | 可用性ゾーン | 既定値 **[なし]** のままにします。 |
    | 仮想ネットワークの選択    |    **[既存のものを使用]** を選択します。    |
    | 仮想ネットワーク    |    **[myAzFwVNet]** を選択します。    |
    | パブリック IP アドレス    |    **[新しく追加]** を選択し、[名前] に「**myFirewall-ip**」と入力します。    |
    | 強制トンネリング    | 既定値の **[無効]** のままにします。    |
    |||
5. **[Review + create]\(レビュー + 作成\)** を選択します。 **[確認および作成]** ページが表示され、Azure によって構成が検証されます。

6. "**証に成功しました**" というメッセージが表示されたら、 **[作成]** を選択します。

## <a name="enable-firewall-logs"></a>ファイアウォール ログの有効化

このセクションでは、ファイアウォールでログを有効にします。

1. Azure portal で、左側のメニューから **[すべてのリソース]** を選択します。

2. リソースの一覧で、ファイアウォール **myAzureFirewall** を選択します。

3. ファイアウォール設定の **[監視]** で、 **[診断設定]** を選択します。

4. [診断設定] で、 **[+ 診断設定の追加]** を選択します。

5. **[診断設定]** で、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | 診断設定の名前 | 「**myDiagSetting**」と入力します。 |
    | カテゴリの詳細 | |
    | log | **AzureFirewallApplicationRule** および **AzureFirewallNetworkRule** を選択します。 |
    | 宛先の詳細 | **[Log Analytics への送信]** を選択します。 |
    | サブスクリプション | サブスクリプションを選択します。 |
    | Log Analytics ワークスペース | Log Analytics ワークスペースを選択します。 |

6. **[保存]** を選択します。

## <a name="create-azure-sql-database"></a>Azure SQL データベースの作成

このセクションでは、プライベート SQL Database を作成します。

1. Azure portal の画面の左上で、 **[リソースの作成]**  >  **[データベース]**  >  **[SQL データベース]** を選択します。

2. **[SQL データベースの作成 - 基本]** で、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | **プロジェクトの詳細** | |
    | サブスクリプション | サブスクリプションを選択します。 |
    | Resource group | **[myResourceGroup]** を選択します。 このリソース グループは、前のセクションで作成しました。|
    | **データベースの詳細** |  |
    | データベース名  | 「**mydatabase**」と入力します。  |
    | サーバー | **[新規作成]** を選択して、次の情報を入力します。    |
    | サーバー名 | 「**mydbserver**」と入力します。 この名前が使用済みの場合は、一意の名前を入力します。   |
    | サーバー管理者のログイン | 任意の名前を入力します。 |
    | Password    |    任意のパスワードを入力します。    |
    | パスワードの確認 | パスワードを再入力します    |
    | 場所    | **[(米国) 米国中南部]** を選択します。    |
    | SQL エラスティック プールを使用しますか?    | 既定値 **[なし]** のままにします。 |
    | コンピューティングとストレージ | 既定値の **[汎用第 5 世代、2 仮想コア、32 GB ストレージ]** のままにします。 |
    |||

3. **[Review + create]\(レビュー + 作成\)** を選択します。 **[確認および作成]** ページが表示され、Azure によって構成が検証されます。

4. "**証に成功しました**" というメッセージが表示されたら、 **[作成]** を選択します。

## <a name="create-private-endpoint"></a>プライベート エンドポイントの作成

このセクションでは、前のセクションの Azure SQL データベース用のプライベート エンドポイントを作成します。

1. Azure portal で、左側のメニューから **[すべてのリソース]** を選択します。

2. サービスの一覧から、Azure SQL サーバー **mydbserver** を選択します。  別のサーバー名を使用した場合は、その名前を選択します。

3. サーバー設定で、 **[セキュリティ]** にある **[プライベート エンドポイント接続]** を選択します。

4. **[+ プライベート エンドポイント]** を選択します。

5. **[プライベート エンドポイントの作成]** の **[基本]** タブで、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | **プロジェクトの詳細** | |
    | サブスクリプション | サブスクリプションを選択します。 |
    | Resource group | **[myResourceGroup]** を選択します。 |
    | **インスタンスの詳細** | |
    | 名前 | 「**SQLPrivateEndpoint**」と入力します。 |
    | リージョン | **[(米国) 米国中南部]** を選択します。 |

6. **[リソース]** タブを選択するか、 **[次へ: リソース]** がページの下部にあるので、これを選択します。

7. **[リソース]** タブで、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | 接続方法 | **[マイ ディレクトリ内の Azure リソースに接続します]** を選択します。 |
    | サブスクリプション | サブスクリプションを選択します。 |
    | リソースの種類 | **[Microsoft.Sql/servers]** を選択します。 |
    | リソース | **mydbserver** または前の手順で作成したサーバーの名前を選択します。
    | ターゲット サブリソース | **[sqlServer]** を選択します。 |

8. **[構成]** タブを選択するか、 **[次へ: 構成]** ボタンがページの下部にあるので、これを選択します。

9. **[構成]** タブで、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | **ネットワーク** | |
    | 仮想ネットワーク | **[myPEVnet]** を選択します。 |
    | Subnet | **[PrivateEndpointSubnet]** を選択します。 |
    | **プライベート DNS の統合** | |
    | プライベート DNS ゾーンとの統合 | **[はい]** を選択します。 |
    | サブスクリプション | サブスクリプションを選択します。 |
    | プライベート DNS ゾーン | 既定値の **privatelink.database.windows.net** のままにします。 |

10. **[確認と作成]** タブを選択するか、ページの下部にある **[確認と作成]** を選択します。

11. **［作成］** を選択します

12. エンドポイントが作成されたら、 **[セキュリティ]** にある **[ファイアウォールと仮想ネットワーク]** を選択します。

13. **[ファイアウォールと仮想ネットワーク]** で、 **[Azure サービスおよびリソースにこのサーバーへのアクセスを許可する]** の横にある **[はい]** を選択します。

14. **[保存]** を選択します。

## <a name="connect-the-virtual-networks-using-virtual-network-peering"></a>仮想ネットワーク ピアリングを使用して仮想ネットワークを接続する

このセクションでは、ピアリングを使用して仮想ネットワーク **myVMVNet** および **myPEVNet** を **myAzFwVNet** に接続します。 **myVMVNet** および **myPEVNet** 間の直接接続はありません。

1. ポータルの検索バーに、「**myAzFwVNet**」と入力します。

2. **[設定]** メニューの **[ピアリング]** を選択し、 **[+ 追加]** を選択します。

3. **[ピアリングの追加]** で、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | myAzFwVNet からリモート仮想ネットワークへのピアリングの名前 | 「**myAzFwVNet-to-myVMVNet**」と入力します。 |
    | **ピアの詳細** |  |
    | 仮想ネットワークのデプロイ モデル  | 既定値の **[Resource Manager]** のままにします。  |
    | リソース ID を知っている | オフのままにします。    |
    | サブスクリプション | サブスクリプションを選択します。    |
    | 仮想ネットワーク | **[myVMVNet]** を選択します。 |
    | リモート仮想ネットワークから myAzFwVNet へのピアリングの名前    |    「**myVMVNet-to-myAzFwVNet**」と入力します。    |
    | **構成** | |
    | **仮想ネットワーク アクセス設定の構成** | |
    | myAzFwVNet からリモート仮想ネットワークへの仮想ネットワーク アクセスを許可する | 既定値 **[有効]** のままにします。    |
    | リモート仮想ネットワークから myAzFwVNet への仮想ネットワーク アクセスを許可する    | 既定値 **[有効]** のままにします。    |
    | **トラフィック転送設定の構成** | |
    | リモート仮想ネットワークから myAzFwVNet へのトラフィック転送を許可する    | **[Enabled]** を選択します。 |
    | myAzFwVNet からリモート仮想ネットワークへのトラフィック転送の名前 | **[Enabled]** を選択します。 |
    | **ゲートウェイ転送設定の構成** | |
    | ゲートウェイ転送を許可する | オフのままにします |
    |||

4. **[OK]** を選択します。

5. **[+ 追加]** を選択します。

6. **[ピアリングの追加]** で、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | myAzFwVNet からリモート仮想ネットワークへのピアリングの名前 | 「**myAzFwVNet-to-myPEVNet**」と入力します。 |
    | **ピアの詳細** |  |
    | 仮想ネットワークのデプロイ モデル  | 既定値の **[Resource Manager]** のままにします。  |
    | リソース ID を知っている | オフのままにします。    |
    | サブスクリプション | サブスクリプションを選択します。    |
    | 仮想ネットワーク | **[myPEVNet]** を選択します。 |
    | リモート仮想ネットワークから myAzFwVNet へのピアリングの名前    |    「**myPEVNet-to-myAzFwVNet**」と入力します。    |
    | **構成** | |
    | **仮想ネットワーク アクセス設定の構成** | |
    | myAzFwVNet からリモート仮想ネットワークへの仮想ネットワーク アクセスを許可する | 既定値 **[有効]** のままにします。    |
    | リモート仮想ネットワークから myAzFwVNet への仮想ネットワーク アクセスを許可する    | 既定値 **[有効]** のままにします。    |
    | **トラフィック転送設定の構成** | |
    | リモート仮想ネットワークから myAzFwVNet へのトラフィック転送を許可する    | **[Enabled]** を選択します。 |
    | myAzFwVNet からリモート仮想ネットワークへのトラフィック転送の名前 | **[Enabled]** を選択します。 |
    | **ゲートウェイ転送設定の構成** | |
    | ゲートウェイ転送を許可する | オフのままにします |

7. **[OK]** を選択します。

## <a name="link-the-virtual-networks-to-the-private-dns-zone"></a>仮想ネットワークをプライベート DNS ゾーンにリンクする

このセクションでは、仮想ネットワーク **myVMVNet** および **myAzFwVNet** をプライベート DNS ゾーン **privatelink.database.windows.net** にリンクします。 このゾーンは、プライベート エンドポイントを作成したときに作成されました。 

このリンクは、VM とファイアウォールで、データベースの FQDN をそのプライベート エンドポイント アドレスに解決するために必要です。 仮想ネットワーク **myPEVNet** は、プライベート エンドポイントが作成されたときに自動的にリンクされました。

>[!NOTE]
>VM とファイアウォールの仮想ネットワークをプライベート DNS ゾーンにリンクしない場合でも、VM とファイアウォールの両方で SQL Server FQDN を解決できます。 それらはパブリック IP アドレスに解決されます。

1. ポータルの検索バーに、「**privatelink.database**」と入力します。

2. 検索結果から **privatelink.database.windows.net** を選択します。

3. **[設定]** で、 **[仮想ネットワーク リンク]** を選択します。

4. **[+ 追加]** を選択します。

5. **[仮想ネットワーク リンクの追加]** で、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | リンク名 | 「**Link-to-myVMVNet**」と入力します。 |
    | **仮想ネットワークの詳細** |  |
    | 仮想ネットワークのリソース ID を知っている  | オフのままにします。  |
    | サブスクリプション | サブスクリプションを選択します。    |
    | 仮想ネットワーク | **[myVMVNet]** を選択します。 |
    | **CONFIGURATION** | |
    | 自動登録を有効にする | オフのままにします。    |


6. **[OK]** を選択します。

## <a name="configure-an-application-rule-with-sql-fqdn-in-azure-firewall"></a>Azure Firewall で SQL FQDN を使用してアプリケーション ルールを構成する

このセクションでは、**myVM** と、SQL Server のプライベート エンドポイントである **mydbserver.database.windows.net** の間で通信を許可するアプリケーション ルールを構成します。 

このルールでは、前の手順で作成したファイアウォール経由の通信を許可します。

1. ポータルの検索バーに、「**myAzureFirewall**」と入力します。

2. 検索結果から **myAzureFirewall** を選択します。

3. **myAzureFirewall** の概要で、 **[設定]** にある **[ルール]** を選択します。

4. **[アプリケーション ルール コレクション]** タブを選択します。

5. **[+ アプリケーション ルール コレクションの追加]** を選択します。

6. **[アプリケーション ルール コレクションの追加]** で、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | 名前 | 「**SQLPrivateEndpoint**」と入力します。 |
    | 優先度 | 「**100**」と入力します。 |
    | アクション | 「**Allow**」と入力します。 |
    | **ルール** |  |
    | **FQDN タグ** | |
    | 名前  | 空白のままにします。  |
    | 変換元の型 | 既定値の **[IP アドレス]** のままにします。    |
    | source | 空白のままにします。 |
    | FQDN のタグ | 既定値の **[0 個選択済み]** のままにします。 |
    | **ターゲット FQDN** | |
    | 名前 | 「**SQLPrivateEndpoint**」と入力します。    |
    | 変換元の型 | 既定値の **[IP アドレス]** のままにします。 |
    | source | 「**10.1.0.0/16**」を入力します。 |
    | プロトコル:Port | 「**mssql:1433**」と入力します。 |
    | ターゲットの FQDN | 「**mydbserver.database.windows.net**」と入力します。 |
    |||

7. **[追加]** を選択します。

## <a name="route-traffic-between-the-virtual-machine-and-private-endpoint-through-azure-firewall"></a>仮想マシンとプライベート エンドポイント間のトラフィックを Azure Firewall 経由でルーティングする

仮想ネットワーク **myVMVNet** および **myPEVNet** の間には、仮想ネットワーク ピアリングを直接的に作成しませんでした。 仮想マシン **myVM** には、作成したプライベート エンドポイントへのルートがありません。 

このセクションでは、カスタム ルートを含むルート テーブルを作成します。 

このルートは、**myVM** サブネットから、仮想ネットワーク **myPEVNet** のアドレス空間に Azure Firewall 経由でトラフィックを送信します。

1. Azure portal のメニューまたは **[ホーム]** ページで **[リソースの作成]** を選択します。

2. 検索ボックスに「**route table**」と入力し、**Enter** キーを押します。

3. **[ルート テーブル]** 、 **[作成]** の順に選択します。

4. **[ルート テーブルの作成]** ページで、次の表を使用してルート テーブルを構成します。

    | 設定 | 値 |
    | ------- | ----- |
    | **プロジェクトの詳細** | |
    | サブスクリプション | サブスクリプションを選択します。 |
    | Resource group | **[myResourceGroup]** を選択します。  |
    | **インスタンスの詳細** |  |
    | リージョン | **[米国中南部]** を選択します。 |
    | 名前 | 「**VMsubnet-to-AzureFirewall**」と入力します。 |
    | ゲートウェイのルートを伝達する | このため、 **[いいえ]** を選択します。 |

5. **[Review + create]\(レビュー + 作成\)** を選択します。 **[確認および作成]** ページが表示され、Azure によって構成が検証されます。

6. "**証に成功しました**" というメッセージが表示されたら、 **[作成]** を選択します。

7. デプロイが完了したら、 **[リソースに移動]** を選択します。

8. **[設定]** で、**[ルート]** を選択します。

9. **[+ 追加]** を選択します。

10. **[ルートの追加]** ページで、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | ルート名 | 「**myVMsubnet-to-privateendpoint**」と入力します。 |
    | アドレス プレフィックス | 「**10.2.0.0/16**」と入力します。  |
    | ネクストホップの種類 | **[仮想アプライアンス]** を選択します。 |
    | 次ホップ アドレス | 「**10.0.0.4**」と入力します。 |

11. **[OK]** を選択します。

12. **[設定]** で、 **[サブネット]** を選択します。

13. **[+ 関連付け]** を選択します。

14. **[サブネットの関連付け]** ページで、次の情報を入力または選択します。

    | 設定 | 値 |
    | ------- | ----- |
    | 仮想ネットワーク | **[myVMVNet]** を選択します。 |
    | Subnet | **[VMSubnet]** を選択します。  |

15. **[OK]** を選択します。

## <a name="connect-to-the-virtual-machine-from-your-client-computer"></a>クライアント コンピューターから仮想マシンに接続する

次のように、インターネットから VM **myVm** に接続します。

1. ポータルの検索バーに、「**myVm-ip**」と入力します。

2. 検索結果から **myVM-ip** を選択します。

3. **[IP アドレス]** の値をコピーするか、メモします。

4. Windows 10 を使用している場合は、PowerShell を使用して次のコマンドを実行します。 それ以外の Windows クライアント バージョンの場合は、[Putty](https://www.putty.org/) などの SSH クライアントを使用します。

* **username** は、VM の作成時に入力した管理者ユーザー名に置き換えます。

* **IPaddress** は、前の手順の IP アドレスに置き換えます。

    ```bash
    ssh username@IPaddress
    ```

5. **myVm** の作成時に定義したパスワードを入力します。

## <a name="access-sql-server-privately-from-the-virtual-machine"></a>仮想マシンから SQL Server にプライベートにアクセスする

このセクションでは、プライベート エンドポイントを使用して SQL Database にプライベートに接続します。

1. 「`nslookup mydbserver.database.windows.net`」と入力します
    
    次のようなメッセージが返されます。

    ```bash
    Server:         127.0.0.53
    Address:        127.0.0.53#53

    Non-authoritative answer:
    mydbserver.database.windows.net       canonical name = mydbserver.privatelink.database.windows.net.
    Name:   mydbserver.privatelink.database.windows.net
    Address: 10.2.0.4
    ```

2. [SQL Server コマンドライン ツール](/sql/linux/quickstart-install-connect-ubuntu?view=sql-server-ver15#tools)をインストールします。

3. 次のコマンドを実行して SQL Server に接続します。 前の手順で SQL Server を作成したときに定義したサーバー管理者とパスワードを使用します。

* **\<ServerAdmin>** は、SQL Server の作成時に入力した管理者ユーザー名に置き換えます。

* **\<YourPassword>** は、SQL Server の作成時に入力した管理者パスワードに置き換えます。

    ```bash
    sqlcmd -S mydbserver.database.windows.net -U '<ServerAdmin>' -P '<YourPassword>'
    ```
4. ログインに成功すると、SQL コマンド プロンプトが表示されます。 「**exit**」と入力して **sqlcmd** ツールを終了します。

5. 「**exit**」と入力して **myVM** への接続を閉じます。

## <a name="validate-the-traffic-in-azure-firewall-logs"></a>Azure Firewall ログ内のトラフィックを検証する

1. Azure portal で、 **[すべてのリソース]** を選択し、Log Analytics ワークスペースを選択します。

2. Log Analytics ワークスペースのページで、 **[全般]** にある **[ログ]** を選択します。

3. 青色の **[開始する]** ボタンを選択します。

4. **[サンプル クエリ]** ウィンドウで、 **[すべてのクエリ]** にある **[ファイアウォール]** を選択します。

5. **[アプリケーション ルールのログ データ]** にある **[実行]** ボタンを選択します。

6. ログクエリの出力で、 **[FQDN]** に **mydbserver.database.windows.net** が、 **[RuleCollection]** に **SQLPrivateEndpoint** がそれぞれ表示されていることを確認します。

## <a name="clean-up-resources"></a>リソースをクリーンアップする

リソースを使い終えたら、リソース グループと、それに含まれるすべてのリソースを削除します。

1. ポータルの上部にある **検索** ボックスに「**myResourceGroup**」と入力し、検索結果から **myResourceGroup** を選択します。

1. **[リソース グループの削除]** を選択します。

1. **[TYPE THE RESOURCE GROUP NAME]\(リソース グループ名を入力してください\)** に「**myResourceGroup**」と入力し、 **[削除]** を選択します。

## <a name="next-steps"></a>次のステップ

この記事では、Azure Firewall を使用して仮想マシンとプライベート エンドポイント間のトラフィックを制限するために使用できるさまざまなシナリオについて説明しました。 

Azure Firewall 経由でプライベート リンクを使用して VM に接続し、データベースと安全に通信しました。

プライベート エンドポイントの詳細については、「[Azure プライベート エンドポイントとは](private-endpoint-overview.md)」を参照してください。