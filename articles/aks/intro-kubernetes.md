---
title: Azure Kubernetes Service の概要
description: Azure でコンテナー ベースのアプリケーションを簡単にデプロイして管理するための、Azure Kubernetes Service の機能とメリットついて説明します。
services: container-service
ms.topic: overview
ms.date: 02/09/2021
ms.custom: mvc
ms.openlocfilehash: 009244322b7c6625523bf46382a170577593f4e6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101718371"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Azure Kubernetes Service (AKS) は、複雑さと運用上のオーバーヘッドの多くを Azure にオフロードすることで、Azure でのマネージド Kubernetes クラスターのデプロイを簡素化します。 ホストされた Kubernetes サービスとして、Azure は正常性監視やメンテナンスなどの重要なタスクを自動的に処理します。  

Kubernetes マスターは Azure によって管理されるので、お客様はエージェント ノードの管理と保守のみを行います。 そのため、マネージド Kubernetes サービスである AKS は無料です。料金は、マスターではなく、クラスター内のエージェント ノードに対してのみ発生します。  

Azure portal、Azure CLI、Azure PowerShell、または Resource Manager テンプレートや Terraform などのテンプレート駆動型のデプロイ オプションを使用して、AKS クラスターを作成できます。 AKS クラスターをデプロイすると、Kubernetes マスターとすべてのノードが自動的にデプロイされ、構成されます。 デプロイ プロセス中に、高度なネットワーク、Azure Active Directory 統合、監視などの追加機能も構成できます。 AKS では、Windows Server のコンテナーがサポートされています。

Kubernetes の基礎の詳細については、[AKS における Kubernetes の中心概念][concepts-clusters-workloads]に関するページを参照してください。

開始するには、[Azure portal][aks-portal] または [Azure CLI][aks-cli] を使用して、AKS のクイックスタートを完了します。

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="access-security-and-monitoring"></a>アクセス、セキュリティ、および監視

セキュリティと管理を強化するために、AKS では、Azure Active Directory (Azure AD) と統合し、以下を行うことができます。
* Kubernetes のロールベースのアクセス制御 (Kubernetes RBAC) を使用する。 
* クラスターとリソースの正常性を監視する。

### <a name="identity-and-security-management"></a>ID とセキュリティ管理

クラスター リソースへのアクセスを制限するために、AKS では [Kubernetes RBAC][kubernetes-rbac] がサポートされています。 Kubernetes RBAC を使用すると、Kubernetes のリソースおよび名前空間へのアクセスとアクセス許可を制御できます。  

また、Azure AD と統合するように AKS クラスターを構成することもできます。 Azure AD の統合により、既存の ID とグループ メンバーシップに基づいて、Kubernetes アクセスを構成できます。 Azure AD の既存のユーザーとグループには、統合されたサインオン エクスペリエンスと AKS リソースへのアクセスを提供できます。  

ID の詳細については、[AKS でのアクセスと ID オプション][concepts-identity]に関するページを参照してください。

ご利用の AKS クラスターをセキュリティで保護するには、[Azure Active Directory と AKS の統合][aks-aad]に関するページを参照してください。

### <a name="integrated-logging-and-monitoring"></a>ログ記録と監視の統合

コンテナーの正常性に関する Azure Monitor では、AKS クラスター内のコンテナー、ノード、コン ローラーと、デプロイされているアプリケーションから、メモリとプロセッサのパフォーマンス メトリックを収集します。 コンテナー ログと [Kubernetes マスター ログ][aks-master-logs]の両方を確認できます。 この監視データは、Azure Log Analytics ワークスペースに保存され、Azure portal、Azure CLI、または REST エンドポイントを介して使用できます。

詳細については、[Azure Kubernetes Service コンテナーの正常性の監視][container-health]に関するページを参照してください。

## <a name="clusters-and-nodes"></a>クラスターとノード

AKS ノードは Azure 仮想マシン (VM) で実行されます。 AKS ノードでは、ストレージをノードやポッドに接続し、クラスター コンポーネントをアップグレードして、GPU を使用できます。 AKS では、混在するオペレーティング システムと Windows Server コンテナーをサポートするために複数のノード プールを実行する Kubernetes クラスターがサポートされています。  

Kubernetes クラスター、ノード、ノード プールの機能の詳細については、[AKS における Kubernetes の中心概念][concepts-clusters-workloads]に関する記事をご覧ください。

### <a name="cluster-node-and-pod-scaling"></a>クラスター ノードとポッドのスケーリング

リソースに対する需要は変化するため、サービスを実行するクラスター ノードやポッドの数を、自動的にスケールアップまたはスケールダウンすることができます。 ポッドの水平オートスケーラーとクラスター オートスケーラーの両方を使用できます。 このスケーリング方法では、AKS クラスターは需要に対して自動的に調整され、必要とされるリソースのみを実行します。

詳細については、[Azure Kubernetes Service (AKS) クラスターのスケーリング][aks-scale]に関するページを参照してください。

### <a name="cluster-node-upgrades"></a>クラスター ノードのアップグレード

AKS では、複数の Kubernetes バージョンを提供しています。 AKS で新しいバージョンが使用できるようになると、Azure portal または Azure CLI を使用してクラスターをアップグレードできます。 アップグレード プロセス中、実行中のアプリケーションの中断を最小限に抑えるために、ノードは慎重に切断およびドレインされます。  

ライフ サイクルのバージョンの詳細については、[AKS でサポートされている Kubernetes のバージョン][aks-supported versions]に関するページを参照してください。 アップグレードする方法の手順については、「[Azure Kubernetes Service (AKS) クラスターのアップグレード][aks-upgrade]」を参照してください。

### <a name="gpu-enabled-nodes"></a>GPU 対応ノード

AKS は GPU 対応ノード プールの作成をサポートしています。 Azure は現在、単一または複数の GPU に対応する VM を提供しています。 GPU 対応 VM は、コンピューティング処理やグラフィック処理の負荷が高い視覚化ワークロードを想定して設計されています。

詳細については、[AKS での GPU の使用][aks-gpu]に関するページを参照してください。

### <a name="confidential-computing-nodes-public-preview"></a>コンフィデンシャル コンピューティング ノード (パブリック プレビュー)

AKS では、Intel SGX ベースのコンフィデンシャル コンピューティング ノード プール (DCSv2 VM) の作成がサポーされています。 コンフィデンシャル コンピューティング ノードを使用すると、ハードウェアベースの高信頼実行環境 (エンクレーブ) でコンテナーを実行できます。 コンテナー間の分離を、構成証明を通じたコード整合性と組み合わせることで、多層防御によるコンテナー セキュリティ戦略を支援できます。 コンフィデンシャル コンピューティング ノードでは、機密コンテナー (既存の Docker アプリ) とエンクレーブ対応コンテナーの両方がサポートされています。

詳細については、[AKS のコンフィデンシャル コンピューティング ノード][conf-com-node]に関するページを参照してください

### <a name="storage-volume-support"></a>ストレージ ボリュームのサポート

アプリケーション ワークロードをサポートするため、永続的なデータ用にストレージ ボリュームをマウントできます。 静的ボリュームと動的ボリュームの両方を使用できます。 ストレージ ボリュームを共有すると予想される接続されているポッドの数に応じて、単一ポッドへのアクセスに対応する Azure Disks、または複数のポッドへの同時アクセスに対応する Azure Files によってサポートされたストレージを使用できます。

詳細については、[AKS でのアプリケーションのストレージ オプション][concepts-storage]に関するページを参照してください。

[Azure Disks][azure-disk] または [Azure Files][azure-files] を使用して、動的な永続ボリュームの利用を開始してください。

## <a name="virtual-networks-and-ingress"></a>仮想ネットワークとイングレス

AKS クラスターは、既存の仮想ネットワークにデプロイできます。 この構成では、クラスター内のすべてのポッドは、仮想ネットワーク内の IP アドレスが割り当てられるので、クラスター内の他のポッドや仮想ネットワーク内の他のノードと直接通信できます。 また、ポッドは、ピアリングされた仮想ネットワーク内の他のサービスに接続することも、ExpressRoute またはサイト間 (S2S) VPN 接続を介してオンプレミス ネットワークに接続することもできます。  

詳細については、[AKS のアプリケーションにおけるネットワークの概念][aks-networking]に関するページを参照してください。

### <a name="ingress-with-http-application-routing"></a>HTTP アプリケーション ルーティングを使用するイングレス

HTTP アプリケーション ルーティング アドオンを利用すると、AKS クラスターにデプロイされたアプリケーションへのアクセスが容易になります。 有効にすると、HTTP アプリケーション ルーティング ソリューションによって AKS クラスター内のイングレス コントローラーが構成されます。  

アプリケーションがデプロイされると、パブリック アクセス可能な DNS 名が自動構成されます。 HTTP アプリケーション ルーティングにより、DNS ゾーンが設定され、AKS クラスターと統合されます。 その後は、Kubernetes イングレス リソースを通常どおりデプロイできます。  

イングレス トラフィックの使用を開始するには、「[HTTP アプリケーション ルーティング][aks-http-routing]」を参照してください。

## <a name="development-tooling-integration"></a>開発ツールの統合

Kubernetes には、AKS とシームレスに連携する開発および管理ツールの豊富なエコシステムがあります。 これらのツールには、Helm や Visual Studio Code 用 Kubernetes 拡張機能が含まれます。 これらのツールは、AKS でシームレスに動作します。  

さらに、Azure には、DevOps Starter など、Kubernetes の効率化に役立つツールがいくつか用意されています。  

DevOps Starter では、既存のコードおよび Git リポジトリを Azure に取り込むための簡単なソリューションを提供しています。 DevOps Starter では、次のことが自動的に実行されます。
* Azure リソース (AKS など) を作成する。 
* CI 用のビルド パイプラインを含むリリース パイプラインを Azure DevOps Services に構成する。 
* CD 用のリリース パイプラインをセットアップする。 
* 監視のために Azure Application Insights リソースを生成する。 

詳細については、[DevOps Starter][azure-devops] に関するページを参照してください。

## <a name="docker-image-support-and-private-container-registry"></a>Docker イメージのサポートとプライベート コンテナー レジストリ

AKS は Docker イメージ形式をサポートしています。 Docker イメージのプライベート ストレージのために、AKS を Azure Container Registry (ACR) と統合できます。

プライベート イメージ ストアを作成するには、[Azure Container Registry][acr-docs] に関するページを参照してください。

## <a name="kubernetes-certification"></a>Kubernetes 認定

AKS は、Kubernetes 準拠として CNCF 認定されています。

## <a name="regulatory-compliance"></a>規制に対するコンプライアンス

AKS は、SOC、ISO、PCI DSS、HIPAA に準拠しています。 詳細については、「[Overview of Microsoft Azure compliance][compliance-doc]」 (Microsoft Azure コンプライアンスの概要) を参照してください。

## <a name="next-steps"></a>次のステップ

Azure CLI のクイックスタートで、AKS のデプロイと管理の詳細を学習します。

> [!div class="nextstepaction"]
> [AKS クイックスタート][aks-cli]

<!-- LINKS - external -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[compliance-doc]: https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./azure-ad-integration-cli.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./concepts-network.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: ../dev-spaces/index.yml
[azure-devops]: ../devops-project/overview.md
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../azure-monitor/containers/container-insights-overview.md
[aks-master-logs]: ./view-control-plane-logs.md
[aks-supported versions]: supported-kubernetes-versions.md
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[kubernetes-rbac]: concepts-identity.md#kubernetes-role-based-access-control-kubernetes-rbac
[concepts-identity]: concepts-identity.md
[concepts-storage]: concepts-storage.md
[conf-com-node]: ../confidential-computing/confidential-nodes-aks-overview.md