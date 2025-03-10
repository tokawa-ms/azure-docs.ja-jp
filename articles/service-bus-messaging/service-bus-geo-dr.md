---
title: Azure Service Bus の geo ディザスター リカバリー | Microsoft Docs
description: 地理的リージョンを使用して Azure Service Bus のフェールオーバーとディザスター リカバリーを行う方法
ms.topic: article
ms.date: 02/10/2021
ms.openlocfilehash: 86d35465e5b31514f4d215095932b857ce7dcb35
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2021
ms.locfileid: "100384320"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus の geo ディザスター リカバリー

データ処理リソースの大きな損害をもたらす停止に対する回復力は、多くの企業にとっての要件であり、業界の規制によって要求される場合もあります。 

Azure Service Bus により、1 つのデータセンター内の複数の障害ドメインにまたがるクラスター間で、個々のマシンまたはラック全体の致命的な障害のリスクが既に分散されています。また、透過的な障害検出およびフェールオーバー メカニズムが実装されるため、通常はそのような障害発生時にも、顕著な中断なしに、保証されたサービス レベル内でサービスが動作し続けます。 [可用性ゾーン](../availability-zones/az-overview.md)の有効化オプションを使用して Service Bus 名前空間が作成されている場合、物理的に分離された 3 つの施設間で停止リスクがさらに分散されます。また、サービスには、施設全体の致命的な損失全体に瞬時に対処するための十分な容量が予約されています。 

可用性ゾーンをサポートするすべてアクティブな Azure Service Bus クラスター モデルは、致命的なハードウェア障害またはデータセンター機能全体の致命的な損失が発生した場合の回復性に関して、オンプレミスのメッセー ジブローカー製品よりも優れています。 それでも、広範囲の物理的な破壊によって、これらの手段によっても十分に防御できない致命的な状況が生じるおそれがあります。 

Service Bus geo ディザスター リカバリー機能は、容易にこの大きさの災害から復旧し、アプリケーションの構成を変更することなく、障害が発生した Azure リージョンを完全に破棄できるように設計されています。 Azure リージョンの破棄には、通常、いくつかのサービスが含まれます。この機能は、主に複合アプリケーション構成の整合性を維持するためのものです。 Service Bus Premium SKU では、この機能がグローバルに使用できます。 

geo ディザスター リカバリー機能を使用すると、名前空間 (キュー、トピック、サブスクリプション、フィルター) の構成全体が、確実に、ペアリングされたプライマリ名前空間からセカンダリ名前空間に継続的にレプリケートされるようになります。また、プライマリからセカンダリへの 1 回限りのフェールオーバー移動をいつでも開始できます。 フェールオーバー移動を行うと、名前空間の選択したエイリアス名がセカンダリ名前空間に再指定され、その後にペアリングが解除されます。 フェールオーバーは、開始されるとほぼ瞬時に完了します。 

> [!IMPORTANT]
> この機能を使用すると、同じ構成の操作を瞬時に継続できますが、**キューやトピック サブスクリプションまたは配信不能キューに保持されているメッセージがレプリケートされることはありません**。 キューのセマンティクスを維持するために、このようなレプリケーションでは、メッセージ データのレプリケーションだけでなく、ブローカー内のすべての状態変更のレプリケーションが必要となります。 ほとんどの Service Bus 名前空間では、必要なレプリケーション トラフィックがアプリケーション トラフィックをはるかに超えます。また、高スループット キューでは、ほとんどのメッセージがプライマリから既に削除されていても、引き続きセカンダリにレプリケートされるため、過度に無駄なトラフィックが発生します。 geo ディザスター リカバリーのために選択するさまざまなペアリングに適用される、待機時間の長いレプリケーション ルートでは、待機時間に起因する調整の影響を受けるため、レプリケーション トラフィックがアプリケーション トラフィックに持続的に対応することが不可能な場合もあります。
 
> [!TIP]
> キューとトピック サブスクリプションの内容をレプリケートし、アクティブ/アクティブ構成内の対応する名前空間を操作して停止や災害に対処するには、この geo ディザスター リカバリー機能セットを使用せずに、[レプリケーションのガイダンス](service-bus-federation-overview.md)に従ってください。  

## <a name="outages-and-disasters"></a>故障と災害

"故障" と "災害" の違いに注意してください。 

*故障* とは、Azure Service Bus が一時的に使用不可能になっていることです。メッセージング ストアなどサービスの一部のコンポーネントや、場合によってはデータセンター全体に影響を与えることがあります。 しかし、問題が解決されると、Service Bus は再び使用可能になります。 通常、故障によってメッセージなどのデータが失われることはありません。 故障の例として、データセンターでの電源障害があります。 一時的な問題やネットワークの問題によって、接続が短時間失われるだけの故障もあります。 

"*災害*" は、Service Bus クラスター、Azure リージョン、またはデータ センターの永久または長期間損失として定義されています。 リージョンまたはデータセンターは、再び利用可能になる場合も、ならない場合もあります。また、数時間や数日間の停止になる場合もあります。 このような災害の例としては、火災、洪水、地震があります。 永続的な災害により、メッセージやイベントなどのデータが失われる可能性があります。 ただし、ほとんどの場合、データ センターがバックアップされていればデータが失われることはなく、メッセージを復元できます。

Azure Service Bus の geo ディザスター リカバリー機能は、災害復旧ソリューションです。 この記事で説明する概念とワークフローは、一時的な故障ではなく、災害のシナリオに適用されます。 Microsoft Azure でのディザスター リカバリーの詳細については、[こちらの記事](/azure/architecture/resiliency/disaster-recovery-azure-applications)をご覧ください。   

## <a name="basic-concepts-and-terms"></a>基本的な概念と用語

ディザスター リカバリー機能は、メタデータの災害復旧を実装しており、一次および二次障害復旧の名前空間に依存しています。 geo ディザスター リカバリー機能は、[Premium SKU](service-bus-premium-messaging.md) でのみ使用可能です。 別名を使用して接続を確立するので、接続文字列に変更を加える必要はありません。

この記事では、次の用語を使用します。

-  *エイリアス*: 設定したディザスター リカバリー構成の名前です。 エイリアスは、1 つの不変の完全修飾ドメイン名 (FQDN) の接続文字列を示します。 アプリケーションでは、このエイリアスの接続文字列を使用して名前空間に接続します。 エイリアスを使用すると、フェールオーバーがトリガーされたときに接続文字列の変更が発生することはありません。

-  *プライマリ/セカンダリ名前空間*: エイリアスに対応する名前空間です。 プライマリ名前空間が "アクティブ" となり、メッセージを受け取ります (既存の名前空間の場合もあれば、新しい名前空間の場合もあります)。 セカンダリ名前空間は "パッシブ" で、メッセージを受け取りません。 両者間のメタデータは同期しているため、どちらでもアプリケーション コードや接続文字列を変更せずにメッセージをシームレスに受信できます。 確実にアクティブな名前空間にだけメッセージを送信するためには、エイリアスを使用する必要があります。 

    > [!IMPORTANT]
    > geo ディザスター リカバリー機能を使用するには、プライマリ名前空間とセカンダリ名前空間のサブスクリプションとリソース グループが同じである必要があります。
-  *Metadata*:名前空間に関連付けられているサービスのエンティティ (キュー、トピック、サブスクリプションなど) とそのプロパティです。 自動的にレプリケートされるのはエンティティとその設定だけです。 メッセージはレプリケートされません。

-  *フェールオーバー*:セカンダリの名前空間をアクティブ化するプロセスです。

## <a name="setup"></a>セットアップ

次のセクションは、名前空間同士のペアリングの設定の概要です。

![1][]

まず (新規作成または既存の) プライマリ名前空間と新しいセカンダリ名前空間をペアリングします。 このペアリングによって、接続に使用できる別名が決定されます。 別名を使用するので、接続文字列を変更する必要はありません。 フェールオーバーのペアリングに追加できるのは、新しい名前空間だけです。 

1. プライマリ名前空間を作成します。
1. サブスクリプションにセカンダリ名前空間を作成し、プライマリ名前空間を持つリソース グループを別のリージョンに作成します。 この手順は省略可能です。 セカンダリ名前空間は、次の手順でペアリングを作成しているときに作成できます。 
1. Azure portal で、プライマリ名前空間に移動します。
1. 左側のメニューの **[geo リカバリー]** を選択し、ツールバーの **[ペアリングの開始]** を選択します。 

    :::image type="content" source="./media/service-bus-geo-dr/primary-namspace-initiate-pairing-button.png" alt-text="プライマリ名前空間からのペアリングの開始":::    
1. **[ペアリングの開始]** ページで、これらの手順を実行します。
    1. サブスクリプションに既存のセカンダリ名前空間を選択するか、新規作成し、プライマリ名前空間を持つリソース グループを選択します。 この例では、既存の名前空間がセカンダリ名前空間として使用されています。  
    1. **[エイリアス]** には、Geo DR のペアリングのエイリアスを入力します。 
    1. そのうえで **[Create]\(作成\)** を選択します。 

        :::image type="content" source="./media/service-bus-geo-dr/initiate-pairing-page.png" alt-text="セカンダリ名前空間を選択する":::        
1. 次の図に示すように、 **[Service Bus Geo DR のエイリアス]** ページが表示されます。 左側のメニューで **[geo リカバリー]** を選択して、[プライマリ名前空間] ページから **[Geo DR のエイリアス]** ページに移動することもできます。 

    :::image type="content" source="./media/service-bus-geo-dr/service-bus-geo-dr-alias-page.png" alt-text="[Service Bus Geo DR のエイリアス] ページ":::
1. **[Geo DR のエイリアス]** ページの左側のメニューで **[共有アクセス ポリシー]** を選択して、エイリアスのプライマリ接続文字列にアクセスします。 この接続文字列は、プライマリまたはセカンダリ名前空間への接続文字列を直接使用する代わりに使用します。 最初、エイリアスは、プライマリ名前空間を指しています。
1. **[概要]** ページに切り替えます。 次のアクションを実行できます。 
    1. プライマリ名前空間とセカンダリ名前空間の間のペアリングを解除する。 ツールバーの **[ペアリングの解除]** を選択します。 
    1. セカンダリ名前空間に手動でフェールオーバーする。 
        1. ツールバーの **[フェールオーバー]** を選択します。 
        1. エイリアスを入力して、セカンダリ名前空間にフェールオーバーすることを確認します。 
        1. **[Safe Failover]\(安全なフェールオーバー\)** オプションをオンにして、セカンダリ名前空間に安全にフェールオーバーします。 この機能により、セカンダリに切り替える前に、保留中の Geo DR レプリケーションが確実に完了しているようになります。 
        1. その後、 **[フェールオーバー]** を選択します。 
        
            :::image type="content" source="./media/service-bus-geo-dr/failover-page.png" alt-text="{alt-text}":::
    
            > [!IMPORTANT]
            > フェールオーバーによって、セカンダリ名前空間がアクティブ化され、geo ディザスター リカバリーのペアリングからプライマリ名前空間が削除されます。 新しい geo ディザスター リカバリーのペアを使用するには、別の名前空間を作成してください。 

1. 最後に、フェールオーバーが必要であるかどうかを検出する監視機構を追加する必要があります。 ほとんどの場合、このサービスは大きなエコシステムの一部分であるため、自動フェールオーバーが実行できることはまれです。フェールオーバーは、他のサブシステムやインフラストラクチャと同期して実行しなければならないケースが多いためです。

### <a name="service-bus-standard-to-premium"></a>Servce Bus Standard から Premium へ
[Azure Service Bus Standard 名前空間を Azure Service Bus Premium に移行済み](service-bus-migrate-standard-premium.md)の場合、既存のエイリアス (つまり、Service Bus Standard 名前空間の接続文字列) を使用して、**PS/CLI** または **REST API** を介したディザスター リカバリー構成を作成する必要があります。

これは、移行中に、Azure Service Bus Standard 名前空間の接続文字列/DNS 名自体が Azure Service Bus Premium 名前空間のエイリアスになるためです。

クライアント アプリケーションでは、このエイリアス (つまり、Azure Service Bus Standard 名前空間の接続文字列) を利用して、ディザスター リカバリーのペアリングが設定されている Premium 名前空間に接続する必要があります。

ポータルを使用してディザスター リカバリー構成を設定する場合、この注意事項を気にする必要はありません。


## <a name="failover-flow"></a>フェールオーバーの流れ

フェールオーバーはお客様によって (コマンドを使用して明示的に、またはコマンドをトリガーするビジネス ロジックを所有しているクライアントを通して) 手動でトリガーされ、Azure によってトリガーされることはありません。 これにより、Azure のバックボーン上での故障に対するソリューションの完全な所有権と可視性がお客様に付与されます。

![4][]

フェールオーバーがトリガーされると:

1. "***エイリアス***" 接続文字列がセカンダリ Premium 名前空間を指すように更新されます。

2. クライアント (送信側と受信側) がセカンダリ名前空間に自動的に接続されます。

3. プライマリ Premium 名前空間とセカンダリ Premium 名前空間の間の既存のペアリングが解除されます。

フェールオーバーが開始されると:

1. 別の故障が発生した場合は、もう一度フェールオーバーしたいと考えます。 そのため、別のパッシブな名前空間を設定して、ペアリングを更新します。 

2. 以前のプライマリ名前空間が再び利用可能になったら、以前のプライマリ名前空間からメッセージをプルします。 以後、通常のメッセージングにその名前空間を geo リカバリーのセットアップ外で使用するか、または、以前のプライマリ名前空間を削除します。

> [!NOTE]
> サポートされるのは、フェール フォワードのセマンティクスだけです。 このシナリオでは、フェールオーバー後、新しい名前空間との間で再度ペアリングを行います。 フェールバックはサポートされません (SQL クラスターでのフェールバックなど)。 

フェールオーバーは、監視システムを使用して自動化できるほか、独自に構築した監視ソリューションを使用して自動化することもできます。 ただしそのような自動化は、特別な計画と作業が伴い、この記事で取り上げる範囲を超えています。

![2][]

## <a name="management"></a>管理

間違ったリージョンをペアリングしたなど、初期設定にミスがあった場合、2 つの名前空間のペアリングはいつでも解除することができます。 ペアリングした名前空間を通常の名前空間として使用する必要がある場合、エイリアスは削除してください。

## <a name="use-existing-namespace-as-alias"></a>既存の名前空間をエイリアスとして使用する

プロデューサーとコンシューマーの接続を変更できないシナリオでは、お使いの名前空間名をエイリアス名として再利用することができます。 [こちらで GitHub のサンプル コード](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name)をご覧ください。

## <a name="samples"></a>サンプル

[GitHub のサンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/)には、フェールオーバーの設定と開始の方法が紹介されています。 サンプルで紹介されている概念は次のとおりです。

- Azure Resource Manager と Service Bus を使用して geo ディザスター リカバリーを設定し、有効にするために Azure Active Directory で必要となる .NET のサンプルと設定。
- サンプル コードを実行するために必要な手順。
- 既存の名前空間をエイリアスとして使用する方法。
- 代替方法として PowerShell または CLI から geo ディザスター リカバリーを有効にするための手順。
- エイリアスを利用し、現在のプライマリまたはセカンダリ名前空間との間で行う[送受信](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1)。

## <a name="considerations"></a>考慮事項

このリリースでは次の考慮事項にご注意ください。

1. フェールオーバー計画では、時間的要因も考慮する必要があります。 たとえば、接続の喪失時間が 15 ～ 20 分を超えた場合にフェールオーバー開始の判断を下すことが考えられます。

2. レプリケートされるデータが存在しないということは、現在アクティブなセッションがレプリケートされないことを意味します。 また、重複の検出やスケジュールされたメッセージが正しく機能しない可能性があります。 新しいセッションやスケジュールされた新しいメッセージ、新しい重複については正しく機能します。 

3. 複雑な分散インフラストラクチャのフェールオーバーは、少なくとも 1 回は[リハーサル](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan)を行うようお勧めします。

4. エンティティの同期には、ある程度時間がかかる場合があります (1 分あたり約 50 ～ 100 エンティティ)。 サブスクリプションやルールもエンティティとしてカウントされます。

### <a name="availability-zones"></a>可用性ゾーン

Service Bus Premium SKU では、同じ Azure リージョン内に障害から分離された場所を提供する [Availability Zones](../availability-zones/az-overview.md) がサポートされています。 Service Bus は、メッセージング ストアの 3つのコピー (1つのプライマリと 2つのセカンダリ) を管理します。 Service Bus は、データ操作および管理操作のために 3つのコピーをすべて同期します。 プライマリ コピーがフェイルした場合には、セカンダリ コピーの 1つをプライマリに昇格させ、ダウンタイムを発生させません。 アプリケーションが Service Bus からの一時的な切断を認識した場合には、SDK の再試行ロジックによって Service Bus に自動で再接続します。 

可用性ゾーンを使用すると、メタデータとデータ (メッセージ) の両方が、可用性ゾーン内のデータ センター間でレプリケートされます。 

> [!NOTE]
> Azure Service Bus Premium に対する Availability Zones のサポートは、可用性ゾーンが利用可能な [Azure リージョン](../availability-zones/az-region.md)でのみ利用できます。でのみ利用できます。

Azure Portal を使用して、新しい名前空間でのみ Availability Zones を有効にすることができます。 Service Bus では、既存の名前空間の移行はサポートされていません。 名前空間でゾーン冗長を有効にした後に、無効にすることはできません。

![3][]

## <a name="private-endpoints"></a>プライベート エンドポイント
このセクションでは、プライベート エンドポイントを使用する名前空間で geo ディザスター リカバリーを使用する場合のその他の考慮事項について説明します。 Service Bus 全般でのプライベート エンドポイントの使用については、[Azure Service Bus と Azure Private Link との統合](private-link-service.md)に関する記事を参照してください。

### <a name="new-pairings"></a>新しいペアリング
プライベート エンドポイントがあるプライマリ名前空間と、プライベート エンドポイントがないセカンダリ名前空間とのペアリングを作成しようとすると、ペアリングは失敗します。 ペアリングは、プライマリとセカンダリの両方の名前空間にプライベート エンドポイントがある場合にのみ成功します。 プライマリおよびセカンダリ名前空間と、プライベート エンドポイントが作成される仮想ネットワークで同じ構成を使用することをお勧めします。 

> [!NOTE]
> プライベート エンドポイントがあるプライマリ名前空間と任意のセカンダリ名前空間を組み合わせようとすると、検証プロセスで、セカンダリ名前空間にプライベート エンドポイントが存在するかどうかのみが確認されます。 エンドポイントが機能するかどうか、またはフェールオーバー後に機能するかどうかは確認されません。 フェールオーバー後に、プライベート エンドポイントがあるセカンダリ名前空間が予期したとおりに機能することをご自身で確認する必要があります。
>
> プライベート エンドポイント構成が同じであることをテストするには、仮想ネットワークの外部からセカンダリ名前空間に[キューの取得](/rest/api/servicebus/stable/queues/get)要求を送信し、サービスからエラー メッセージを受信したことを確認します。

### <a name="existing-pairings"></a>既存のペアリング
プライマリ名前空間とセカンダリ名前空間のペアリングが既に存在する場合、プライマリ名前空間でのプライベート エンドポイントの作成は失敗します。 解決するには、まずセカンダリ名前空間にプライベート エンドポイントを作成し、次にプライマリ名前空間用に作成します。

> [!NOTE]
> セカンダリ名前空間に対して許可されるのは読み取り専用アクセスですが、プライベート エンドポイント構成の更新は許可されます。 

### <a name="recommended-configuration"></a>推奨される構成
アプリケーションと Service Bus のディザスター リカバリー構成を作成する場合は、アプリケーションのプライマリ インスタンスとセカンダリ インスタンスの両方をホストする仮想ネットワークに対して、プライマリとセカンダリの両方の Service Bus 名前空間にプライベート エンドポイントを作成する必要があります。

たとえば、2 つの仮想ネットワーク (VNET-1、VNET-2)、プライマリ名前空間とセカンダリ名前空間 (ServiceBus-Namespace1-Primary、ServiceBus-Namespace2-Secondary) があるとします。 次の手順を実行する必要があります。 

- ServiceBus-Namespace1-Primary に、VNET-1 および VNET-2 のサブネットを使用する 2 つのプライベート エンドポイントを作成します。
- ServiceBus-Namespace2-Secondary に、VNET-1 と VNET-2 の同じサブネットを使用する 2 つのプライベート エンドポイントを作成します。 

![プライベート エンドポイントと仮想ネットワーク](./media/service-bus-geo-dr/private-endpoints-virtual-networks.png)


このアプローチの利点は、Service Bus 名前空間から独立したアプリケーション レイヤーでフェールオーバーが発生するようにできることです。 以下のようなシナリオが考えられます。 

**アプリケーションのみのフェールオーバー:** ここでは、アプリケーションは VNET-1 には存在しませんが、VNET 2 に移行します。 VNET-1 と VNET 2 の両方で、プライマリとセカンダリの両方の名前空間に対して両方のプライベート エンドポイントが構成されているため、アプリケーションは動作します。 

**Service Bus 名前空間のみのフェールオーバー**: ここでも、プライマリとセカンダリの両方の名前空間に対して、両方の仮想ネットワークで両方のプライベート エンドポイントが構成されているため、アプリケーションは動作します。 

> [!NOTE]
> 仮想ネットワークの geo ディザスター リカバリーに関するガイダンスについては、「[Virtual Network - ビジネス継続性](../virtual-network/virtual-network-disaster-recovery-guidance.md)」を参照してください。

## <a name="next-steps"></a>次のステップ

- geo ディザスター リカバリーの [REST API リファレンス](/rest/api/servicebus/stable/disasterrecoveryconfigs)を確認する
- geo ディザスター リカバリーの[ GitHub のサンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2)を実行する
- geo ディザスター リカバリーの[別名にメッセージを送信するサンプル](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1)を確認する

Service Bus メッセージングの詳細については、次の記事をご覧ください。

* [Service Bus のキュー、トピック、サブスクリプション](service-bus-queues-topics-subscriptions.md)
* [Service Bus キューの使用](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus のトピックとサブスクリプションの使用方法](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [Rest API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png
