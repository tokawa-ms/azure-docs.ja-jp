---
ms.date: 02/16/2021
ms.topic: reference-architecture
author: kriation
title: Azure Spring Cloud の参照アーキテクチャ
ms.author: akaleshian
ms.service: spring-cloud
description: この参照アーキテクチャは、Azure Spring Cloud を使用するための一般的なエンタープライズ ハブとスポーク設計を使用した基盤です。
ms.openlocfilehash: c87462c8d6ab86299b6202acaa23f93d19240e0e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101735490"
---
# <a name="azure-spring-cloud-reference-architecture"></a>Azure Spring Cloud の参照アーキテクチャ

この参照アーキテクチャは、Azure Spring Cloud を使用するための一般的なエンタープライズ ハブとスポーク設計を使用した基盤です。 この設計では、Azure Spring Cloud はハブでホストされている共有サービスに依存する単一のスポークにデプロイされます。 アーキテクチャは、「[Microsoft Azure Well-Architected Framework][16]」の原則を達成するためのコンポーネントで構築されています。

このアーキテクチャの実装については、GitHub にある [Azure Spring Cloud の参照アーキテクチャ][10] リポジトリを参照してください。

このアーキテクチャのデプロイ オプションには、Azure Resource Manager (ARM)、Terraform、および Azure CLI が含まれます。 このリポジトリ内の成果物により、ご利用の環境に合わせてカスタマイズできる基礎が提供されます。 Azure Firewall や Application Gateway などのリソースを別のリソース グループまたはサブスクリプションにグループ化することができます。 このグループ化は、IT インフラストラクチャ、セキュリティ、ビジネス アプリケーション チームなどのさまざまな機能を別々に保つのに役立ちます。

## <a name="planning-the-address-space"></a>アドレス空間の計画

Azure Spring Cloud には、次の 2 つの専用サブネットが必要です。

* サービス ランタイム
* Spring Boot アプリケーション

これらのサブネットにはそれぞれ専用のクラスターが必要です。 複数のクラスターで同じサブネットを共有することはできません。 各サブネットの最小サイズは /28 です。 Azure Spring Cloud でサポートできるアプリケーション インスタンスの数は、サブネットのサイズによって異なります。 詳細な Virtual Network (VNET) の要件については、「[仮想ネットワークに Azure Spring Cloud をデプロイする][17]」の「[仮想ネットワークの要件][11]」セクションを参照してください。

> [!WARNING]
> 選択するサブネットのサイズは、既存の VNET アドレス空間と重複することはできません。また、ピアリングされた、あるいはオンプレミスのサブネット アドレス範囲と重複することはできません。

## <a name="use-cases"></a>ユース ケース

このアーキテクチャの一般的な用途は次のとおりです。

* ハイブリッド クラウド環境にデプロイされた内部アプリケーション
* 外部に接続するアプリケーション

これらのユース ケースは、そのセキュリティとネットワーク トラフィック規則以外は似ています。 このアーキテクチャは、それぞれの微妙な差異をサポートするように設計されています。

## <a name="private-applications"></a>プライベート アプリケーション

次の一覧では、プライベート アプリケーションのインフラストラクチャ要件について説明します。 これらの要件は、厳しく規制された環境では一般的です。

* コントロール プレーンのトラフィックを除き、パブリック インターネットへの直接のエグレスはありません。
* エグレス トラフィックは、中央のネットワーク仮想アプライアンス (NVA) (Azure Firewall など) を経由する必要があります。
* 保存データは暗号化する必要があります。
* 転送中のデータは暗号化する必要があります。
* DevOps デプロイ パイプラインを使用することができ (Azure DevOps など)、Azure Spring Cloud へのネットワーク接続が必要です。
* Microsoft のゼロ トラスト セキュリティ手法では、シークレット、証明書、および資格情報をセキュリティで保護されたコンテナーに格納する必要があります。 推奨されるサービスは Azure Key Vault です。
* アプリケーション ホストのドメイン ネームサービス (DNS) レコードは、Azure プライベート DNS に格納する必要があります。
* オンプレミスおよびクラウド内のホストの名前解決は、双方向である必要があります。
* 少なくとも 1 つのセキュリティ ベンチマークへの準拠を強制する必要があります。
* Azure サービスの依存関係では、サービス エンドポイントまたはプライベート リンクを介して通信する必要があります。
* Azure Spring Cloud デプロイで管理されているリソース グループは変更できません。
* Azure Spring Cloud デプロイで管理されているサブネットは変更できません。
* サブネットには、Azure Spring Cloud のインスタンスが 1 つだけ必要です。
* [Azure Spring Cloud Config Server][8] を使用してリポジトリから構成プロパティを読み込む場合は、リポジトリがプライベートである必要があります。

次の一覧には、設計を構成するコンポーネントが示されています。

* オンプレミス ネットワーク
  * ドメイン ネーム サービス (DNS)
  * Gateway
* ハブ サブスクリプション
  * Azure Firewall サブネット
  * Application Gateway サブネット
  * 共有サービス サブネット
* 接続されているサブスクリプション
  * 仮想ネットワーク ピア
  * Azure Bastion サブネット

次の一覧では、この参照アーキテクチャでの Azure サービスについて説明します。

* [Azure Spring Cloud][1]: Java ベースの Spring Boot アプリケーションと .NET ベースの [Steeltoe][9] アプリケーションに特化して設計および最適化されたマネージド サービス。

* [Azure Key Vault][2]: Microsoft の ID サービスとコンピューティング リソースに緊密に統合されている、ハードウェアを基盤にした資格情報管理サービス。

* [Azure Monitor][3]: Azure とオンプレミスの両方にデプロイするアプリケーション向けの包括的な監視サービス スイート。

* [Azure Security Center][4]: オンプレミス、複数のクラウド、および Azure にわたるワークロードのために統合されたセキュリティ管理および脅威防止システム。

* [Azure Pipelines][5]: 更新された Spring Boot アプリを Azure Spring Cloud に自動的にデプロイできる、完全な機能を備えた継続的インテグレーションまたは継続的開発 (CI/CD) サービス。

次の図には、上記の要件に対応する、適切に設計されたハブとスポークの設計が示されています。

![プライベート アプリケーションの参照アーキテクチャの図](./media/spring-cloud-reference-architecture/architecture-private.png)

## <a name="public-applications"></a>パブリック アプリケーション

次の一覧では、パブリック アプリケーションのインフラストラクチャ要件について説明します。 これらの要件は、厳しく規制された環境では一般的です。

* イングレス トラフィックは、少なくとも Application Gateway または Azure Front Door で管理する必要があります。
* Azure DDoS Protection Standard を有効にする必要があります。
* コントロール プレーンのトラフィックを除き、パブリック インターネットへの直接のエグレスはありません。
* エグレス トラフィックは、中央のネットワーク仮想アプライアンス (NVA) (Azure Firewall など) を経由する必要があります。
* 保存データは暗号化する必要があります。
* 転送中のデータは暗号化する必要があります。
* DevOps デプロイ パイプラインを使用することができ (Azure DevOps など)、Azure Spring Cloud へのネットワーク接続が必要です。
* Microsoft のゼロ トラスト セキュリティ手法では、シークレット、証明書、および資格情報をセキュリティで保護されたコンテナーに格納する必要があります。 推奨されるサービスは Azure Key Vault です。
* アプリケーション ホストの DNS レコードは、Azure プライベート DNS に格納する必要があります。
* インターネット ルーティング可能なアドレスは、Azure パブリック DNS に格納する必要があります。
* オンプレミスおよびクラウド内のホストの名前解決は、双方向である必要があります。
* 少なくとも 1 つのセキュリティ ベンチマークへの準拠を強制する必要があります。
* Azure サービスの依存関係では、サービス エンドポイントまたはプライベート リンクを介して通信する必要があります。
* Azure Spring Cloud デプロイで管理されているリソース グループは変更できません。
* Azure Spring Cloud デプロイで管理されているサブネットは変更できません。
* サブネットには、Azure Spring Cloud のインスタンスが 1 つだけ必要です。

次の一覧には、設計を構成するコンポーネントが示されています。

* オンプレミス ネットワーク
  * ドメイン ネーム サービス (DNS)
  * Gateway
* ハブ サブスクリプション
  * Azure Firewall サブネット
  * Application Gateway サブネット
  * 共有サービス サブネット
* 接続されているサブスクリプション
  * 仮想ネットワーク ピア
  * Azure Bastion サブネット

次の一覧では、この参照アーキテクチャでの Azure サービスについて説明します。

* [Azure Spring Cloud][1]: Java ベースの Spring Boot アプリケーションと .NET ベースの [Steeltoe][9] アプリケーションに特化して設計および最適化されたマネージド サービス。

* [Azure Key Vault][2]: Microsoft の ID サービスとコンピューティング リソースに緊密に統合されている、ハードウェアを基盤にした資格情報管理サービス。

* [Azure Monitor][3]: Azure とオンプレミスの両方にデプロイするアプリケーション向けの包括的な監視サービス スイート。

* [Azure Security Center][4]: オンプレミス、複数のクラウド、および Azure にわたるワークロードのために統合されたセキュリティ管理および脅威防止システム。

* [Azure Pipelines][5]: 更新された Spring Boot アプリを Azure Spring Cloud に自動的にデプロイできる、完全な機能を備えた継続的インテグレーションまたは継続的開発 (CI/CD) サービス。

* [Azure Application Gateway][6]: レイヤー 7 でトランスポート層セキュリティ (TLS) のオフロード操作を行うアプリケーション トラフィックを担当するロード バランサー。

* [Azure アプリケーション ファイアウォール][7]: 一般的な脆弱性やその悪用からアプリケーションを一元的に保護する Azure Application Gateway の機能。

次の図には、上記の要件に対応する、適切に設計されたハブとスポークの設計が示されています。

![パブリック アプリケーションの参照アーキテクチャの図](./media/spring-cloud-reference-architecture/architecture-public.png)

## <a name="azure-spring-cloud-on-premises-connectivity"></a>Azure Spring Cloud のオンプレミス接続

Azure Spring Cloud で実行されるアプリケーションは、さまざまな Azure、オンプレミス、および外部リソースと通信できます。 ハブとスポークの設計を使用することにより、アプリケーションでは、Express Route またはサイト間仮想プライベート ネットワーク (VPN) を使用して外部またはオンプレミスのネットワークにトラフィックをルーティングできます。

## <a name="azure-well-architected-framework-considerations"></a>Azure Well-Architected Framework に関する考慮事項

[Azure Well-Architected Framework][16] は、強力なインフラストラクチャ基盤を確立する際に従う一連の基本原則です。 このフレームワークには、コストの最適化、オペレーショナル エクセレンス、パフォーマンス効率、信頼性、およびセキュリティというカテゴリが含まれています。

### <a name="cost-optimization"></a>コストの最適化

分散システムの設計の性質上、インフラストラクチャが増加するのが現実です。 この現実により、コストが予想外で制御できないものになります。 Azure Spring Cloud は、需要を満たし、コストを最適化できるようにスケーリングするコンポーネントを使用して構築されています。 このアーキテクチャの中核となるのが、Azure Kubernetes Service (AKS) です。 サービスは、Kubernetes の管理に伴う複雑さと運用上のオーバーヘッドを軽減するように設計されています。これには、クラスターの運用コストの効率が含まれます。

Azure Spring Cloud の単一のインスタンスに、さまざまなアプリケーションとアプリケーションの種類をデプロイできます。 サービスでは、メトリックまたはスケジュールによってトリガーされるアプリケーションの自動スケールがサポートされており、使用率とコスト効率を向上させることができます。

Application Insights と Azure Monitor を使用して、運用コストを削減することもできます。 包括的なログ ソリューションによって提供される可視性により、システムのコンポーネントをリアルタイムにスケーリングするための自動化を実装できます。 また、ログ データを分析し、システムの全体的なコストとパフォーマンスを向上させるために対処できるアプリケーション コードの非効率性を明らかにすることもできます。

### <a name="operational-excellence"></a>オペレーショナル エクセレンス

Azure Spring Cloud は、オペレーショナル エクセレンスの複数の側面に対応しています。 次の一覧に示すように、これらの側面を組み合わせ、確実に運用環境でサービスが効率的に実行されるようにすることができます。

* Azure Pipelines を使用すると、人為的なエラーを回避するのに役立つと同時に、デプロイの信頼性と整合性を確保することができます。
* Azure Monitor と Application Insights を使用して、ログと利用統計情報を格納できます。
  収集されたログとメトリック データを評価して、ご利用のアプリケーションの正常性とパフォーマンスを確かめることができます。 アプリケーション パフォーマンス監視 (APM) は、Java エージェントを使用してサービスに完全に統合されます。 このエージェントで、追加のコードを必要とせずに、デプロイされたすべてのアプリケーションと依存関係を可視化できます。 詳細については、ブログ記事の「[Azure Spring Cloud でアプリケーションと依存関係を簡単に監視する][15]」を参照してください。
* Azure Security Center を使用すると、指定されたデータを分析および評価するためのプラットフォームを提供することによって、アプリケーションで確実にセキュリティを維持することができます。
* サービスでさまざまなデプロイ パターンがサポートされます。 詳細については、「[Azure Spring Cloud でステージング環境を設定する][14]」を参照してください。

### <a name="reliability"></a>[信頼性]

Azure Spring Cloud は、基本コンポーネントとして AKS を使用して設計されています。 AKS ではクラスタリングによって高いレベルの回復性が提供されますが、この参照アーキテクチャには、コンポーネントで障害が発生した場合にアプリケーションの可用性を向上させるためのサービスとアーキテクチャの考慮事項が組み込まれています。

適切に定義されたハブとスポークの設計の上に構築することで、このアーキテクチャの基盤により、確実に複数のリージョンにデプロイできるようになります。 プライベート アプリケーションのユース ケースでは、このアーキテクチャで Azure プライベート DNS を使用して、地理的な障害時に継続的に可用性を確保します。 パブリック アプリケーションのユース ケースでは、Azure Front Door と Azure Application Gateway によって可用性が確保されます。

### <a name="security"></a>セキュリティ

このアーキテクチャのセキュリティは、業界定義のコントロールとベンチマークに準拠することによって対処されます。 このアーキテクチャのコントロールは、[クラウド セキュリティ アライアンス][18] (CSA) による [Cloud Control Matrix][19] (CCM) と、[Center for Internet Security][21] (CIS) による [Microsoft Azure Foundations Benchmark][20] (MAFB) からのものです。 適用されたコントロールでは、ガバナンス、ネットワーク、およびアプリケーション セキュリティの主要なセキュリティ設計原則に焦点を当てています。 ターゲット インフラストラクチャに関連する ID、アクセス管理、およびストレージの設計原則を扱うことはお客様の責任となります。

#### <a name="governance"></a>ガバナンス

このアーキテクチャで対処するガバナンスの主な側面は、ネットワーク リソースの分離による隔離です。 CCM の DCS-08 では、データセンターのイングレスおよびエグレス コントロールが推奨されます。 このコントロールを満たすために、アーキテクチャでは、ネットワーク セキュリティ グループ (NSG) を使用したハブとスポークの設計を使って、リソース間の east-west トラフィックをフィルター処理します。 また、このアーキテクチャでは、ハブの中央サービスとスポークのリソースとの間のトラフィックもフィルター処理します。 このアーキテクチャでは、Azure Firewall のインスタンスを使用して、インターネットと、アーキテクチャ内のリソースとの間のトラフィックを管理します。

次の一覧には、この参照でのデータセンターのセキュリティに対処するコントロールが示されています。

| CSA CCM コントロール ID | CSA CCM コントロール ドメイン |
| :----------------- | :----------------------|
| DCS-08 | データセンター セキュリティの承認されていない人物エントリ |

#### <a name="network"></a>ネットワーク

このアーキテクチャをサポートするネットワーク設計は、従来のハブ アンド スポーク モデルから派生したものです。 この決定により、ネットワーク分離が基本コンストラクトになります。 CCM コントロール IVS-06 では、信頼されている環境と信頼されていない環境の間で、ネットワークと仮想マシン間のトラフィックを制限し、監視することが推奨されます。 このアーキテクチャには、east-west トラフィック用の NSG と、north-south トラフィック用の Azure Firewall の実装によるコントロールが採用されます。 CCM コントロール IPY-04 では、インフラストラクチャでサービス間のデータ交換にセキュリティで保護されたネットワーク プロトコルを使用する必要があることが推奨されます。 このアーキテクチャをサポートするすべての Azure サービスでは、HTTP や SQL の TLS など、標準的なセキュリティで保護されたプロトコルが使用されます。

次の一覧には、この参照でのネットワーク セキュリティに対処する CCM コントロールが示されています。

| CSA CCM コントロール ID | CSA CCM コントロール ドメイン |
| :----------------- | :----------------------|
| IPY-04             | ネットワーク プロトコル      |
| IVS-06             | ネットワークのセキュリティ       |

ネットワークの実装は、MAFB からコントロールを定義することでさらにセキュリティで保護されます。 このコントロールにより、パブリック インターネットから環境へのトラフィックが制限されるようになります。

次の一覧には、この参照でのネットワーク セキュリティに対処する CIS コントロールが示されています。

| CIS コントロール ID | CIS コントロールの説明 |
| :------------- | :---------------------- |
| 6.2 | インターネットからの SSH アクセスが制限されるようにします。 |
| 6.3 | SQL データベースでイングレス 0.0.0.0/0 (任意の IP) が許可されないようにします。 |
| 6.5 | Network Watcher が [有効] になっていることを保証します。 |
| 6.6 | インターネットからの UDP を使用したイングレスが制限されるようにします。 |

Azure Spring Cloud では、セキュリティで保護された環境にデプロイされている場合、Azure から送信される管理トラフィックが必要です。 これを実現するには、「[VNET での Azure Spring Cloud の実行に関するお客様の責任](./spring-cloud-vnet-customer-responsibilities.md)」に一覧表示されているネットワークとアプリケーションの規則を許可する必要があります。

#### <a name="application-security"></a>アプリケーションのセキュリティ

この設計原則では、ID、データ保護、キー管理、およびアプリケーション構成の基本的なコンポーネントについて説明します。 設計上、Azure Spring Cloud にデプロイされたアプリケーションは、機能するのに必要な最小限の特権で実行されます。 サービスを使用すると、データ保護に承認コントロールのセットが直接関連付けられます。 キー管理によって、このレイヤード アプリケーションのセキュリティ アプローチが強化されます。

次の一覧には、この参照でのキー管理に対処する CCM コントロールが示されています。

| CSA CCM コントロール ID | CSA CCM コントロール ドメイン |
| :----------------- | :--------------------- |
| EKM-01 | 暗号化とキー管理のエンタイトルメント |
| EKM-02 | 暗号化とキー管理のキー生成 |
| EKM-03 | 暗号化とキー管理の機密データ保護 |
| EKM-04 | 暗号化とキー管理のストレージとアクセス |

CCM の EKM-02 および EKM-03 では、キーを管理し、暗号化プロトコルを使用して機密データを保護するためのポリシーと手順が推奨されています。 EKM-01 では、すべての暗号化キーに識別可能な所有者が存在し、管理できるようにすることが推奨されています。 EKM-04 では、標準アルゴリズムを使用することが推奨されています。

次の一覧には、この参照でのキー管理に対処する CIS コントロールが示されています。

| CIS コントロール ID | CIS コントロールの説明 |
| :------------- | :---------------------- |
| 8.1 | すべてのキーに有効期限が設定されていることを保証します。 |
| 8.2 | すべてのシークレットに有効期限が設定されていることを確認します。 |
| 8.4 | キー コンテナーが回復可能であることを確認します。 |

CIS コントロール 8.1 および 8.2 では、ローテーションが確実に適用されるように、資格情報に有効期限を設定することが推奨されていいます。 CIS コントロール 8.4 により、ビジネス継続性を維持するためにキー コンテナーの内容を復元できることが保証されます。

アプリケーション セキュリティの側面により、この参照アーキテクチャを使用して Azure の Spring ワークロードをサポートするための基盤が設定されます。

## <a name="next-steps"></a>次のステップ

[Azure Spring Cloud の参照アーキテクチャ][10] リポジトリで提供されている ARM、Terraform、Azure CLI のデプロイを通じて、この参照アーキテクチャについて確認します。

<!-- Reference links in article -->
[1]: ./index.yml
[2]: ../key-vault/index.yml
[3]: ../azure-monitor/index.yml
[4]: ../security-center/index.yml
[5]: /azure/devops/pipelines/
[6]: ../application-gateway/index.yml
[7]: ../web-application-firewall/index.yml
[8]: ./spring-cloud-tutorial-config-server.md
[9]: https://steeltoe.io/
[10]: https://github.com/Azure/azure-spring-cloud-reference-architecture
[11]: ./spring-cloud-tutorial-deploy-in-azure-virtual-network.md#virtual-network-requirements
[12]: ./spring-cloud-vnet-customer-responsibilities.md#azure-spring-cloud-network-requirements
[13]: ./spring-cloud-vnet-customer-responsibilities.md#azure-spring-cloud-fqdn-requirements--application-rules
[14]: ./spring-cloud-howto-staging-environment.md
[15]: https://devblogs.microsoft.com/java/monitor-applications-and-dependencies-in-azure-spring-cloud/
[16]: /azure/architecture/framework/
[17]: ./spring-cloud-tutorial-deploy-in-azure-virtual-network.md#virtual-network-requirements
[18]: https://cloudsecurityalliance.org/
[19]: https://cloudsecurityalliance.org/research/working-groups/cloud-controls-matrix
[20]: https://azure.microsoft.com/resources/cis-microsoft-azure-foundations-security-benchmark/
[21]: https://www.cisecurity.org/