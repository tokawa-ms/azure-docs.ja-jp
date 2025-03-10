---
title: ネットワーク セキュリティの概念および Azure における要件 | Microsoft Docs
description: この記事では、ネットワーク セキュリティの中核をなす概念と要件の基本的な説明と、各分野で Azure が提供している事柄に関する情報を提供します。
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: TomSh
ms.assetid: bedf411a-0781-47b9-9742-d524cf3dbfc1
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/29/2018
ms.author: terrylan
ms.openlocfilehash: 4fb4981925593a22ed65df9d3ed0da5f45062f23
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "100578938"
---
# <a name="azure-network-security-overview"></a>Azure のネットワーク セキュリティの概要

ネットワーク セキュリティは、ネットワーク トラフィックに制御を適用することにより、リソースを未承認のアクセスや攻撃から保護するプロセスとして定義することが可能でした。 その目的は、正当なトラフィックだけを許可することです。 Azure には、アプリケーションとサービスの接続要件をサポートする堅牢なネットワーク インフラストラクチャが組み込まれています。 ネットワーク接続は、Azure に配置されているリソース間、オンプレミスのリソースと Azure でホストされているリソース間、インターネットと Azure 間で可能です。

この記事では、Azure がネットワーク セキュリティの領域で提供しているオプションについて説明します。 以下について説明します。

* Azure のネットワーク
* ネットワーク アクセス制御
* Azure Firewall
* セキュリティで保護されたリモート アクセスとクロスプレミス接続
* 可用性
* 名前解決
* 境界ネットワーク (DMZ) のアーキテクチャ
* Azure の DDoS 保護
* Azure Front Door
* Traffic Manager
* 監視と脅威の検出

## <a name="azure-networking"></a>Azure のネットワーク

Azure では、仮想マシンを Azure Virtual Network に接続する必要があります。 仮想ネットワークは、物理的な Azure ネットワーク ファブリック上に構築される論理的な構築物です。 各仮想ネットワークは、他のすべての仮想ネットワークから分離されています。 これは、自分のデプロイ内のネットワーク トラフィックに他の Azure ユーザーがアクセスすることを防ぐために役立ちます。

詳細情報:

* [Virtual Network の概要](../../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>ネットワーク アクセス制御

ネットワーク アクセス制御は、仮想ネットワーク内の特定のデバイスまたはサブネットとの間の接続を制限する機能です。 ネットワーク アクセス制御の目的は、承認済みのユーザーとデバイスのみが、仮想マシンとサービスにアクセスできるようにすることです。 アクセス制御は、仮想マシンやサービスとの間の接続を許可するか、それとも拒否するかの決定に基づいて行われます。

Azure では、次のように、いくつかの種類のネットワーク アクセス制御がサポートされています。

* ネットワーク層制御
* ルート制御と強制トンネリング
* 仮想ネットワークのセキュリティ アプライアンス

### <a name="network-layer-control"></a>ネットワーク層制御

セキュリティで保護されたデプロイにはいずれも、ある程度のネットワーク アクセス制御が必要です。 ネットワーク アクセス制御の目的は、必要なシステムのみが仮想マシンと通信できるようにすることです。 その他の通信試行はブロックされます。

> [!NOTE]
> Storage のファイアウォールについては、「[Azure Storage のセキュリティの概要](../../storage/blobs/security-recommendations.md)」の記事に説明されています

#### <a name="network-security-rules-nsgs"></a>ネットワーク セキュリティ規則 (NSG)

基本レベルのネットワーク アクセス制御 (IP アドレス、TCP または UDP プロトコルに基づくもの) を必要とする場合には、ネットワーク セキュリティ グループ (NSG) を使用できます。 NSG とは基本的かつステートフルなパケット フィルタリング ファイアウォールであり、[5 タプル](https://www.techopedia.com/definition/28190/5-tuple)に基づいてアクセスを制御します。 NSG には、管理を簡素化し、構成ミスの可能性を低減する機能が含まれます。

* **拡張セキュリティ規則** は、NSG ルールの定義を簡素化し、複数の単純なルールを作成するのではなく、複雑なルールを作成して、同じ結果を達成できます。
* **サービス タグ** は、Microsoft が作成した IP アドレスのグループを表すラベルです。 それらは、ラベルへの包含を定義する条件を満たす IP 範囲を含むように動的に更新されます。 たとえば、東部リージョンのすべての Azure ストレージに適用されるルールを作成する場合は、Storage.EastUS を使用できます。
* **アプリケーション セキュリティ グループ** を使用して、アプリケーション グループにリソースをデプロイし、アプリケーション グループを使用するルールを作成してリソースへのアクセスを制御できます。 たとえば、'Webservers' アプリケーション グループにデプロイした Web サーバーがある場合は、インターネットから 'Webservers' アプリケーション グループ内のすべてのシステムへの 443 トラフィックを許可する規則を作成して NSG に適用できます。

NSG はアプリケーション層検査も、認証済みのアクセス制御も提供しません。

詳細情報:

* [ネットワーク セキュリティ グループ](../../virtual-network/network-security-groups-overview.md)

#### <a name="asc-just-in-time-vm-access"></a>ASC Just In Time VM アクセス

[Azure セキュリティ センター (ASC)](../../security-center/security-center-introduction.md) は、VM の NSG を管理し、適切な Azure ロール ベースのアクセス制御 ([Azure RBAC](../../role-based-access-control/overview.md)) のアクセス許可を持つユーザーがアクセスを要求するまで、VM へのアクセスをロックします。 ユーザーが正常に承認されると、ASC は、選択されたポートに対して指定された時間にわたってアクセスできるように NSG を変更します。 時間切れになると、NSG は、前のセキュリティで保護された状態に戻ります。

詳細情報:

* [Azure Security Center の Just In Time アクセス](../../security-center/security-center-just-in-time.md)

#### <a name="service-endpoints"></a>サービス エンドポイント

サービス エンドポイントは、トラフィックに制御を適用するための別の方法です。 サポートされているサービスとの通信を、直接接続による VNet だけに制限できます。 VNet から特定の Azure サービスへのトラフィックは、Microsoft Azure のバックボーン ネットワーク上にとどまります。  

詳細情報:

* [サービス エンドポイント](../../virtual-network/virtual-network-service-endpoints-overview.md#secure-azure-services-to-virtual-networks)

### <a name="route-control-and-forced-tunneling"></a>ルート制御と強制トンネリング

仮想ネットワークでのルーティング動作を制御する機能はきわめて重要です。 ルーティングを正しく構成しないと、仮想マシンでホストされるアプリケーションとサービスが、未承認のデバイスに接続する可能性があります。これには潜在的な攻撃者が所有および操作するシステムが含まれます。

Azure ネットワークは、仮想ネットワーク上のネットワーク トラフィックのルーティング動作をカスタマイズする機能をサポートしています。 これにより、ユーザーは仮想ネットワーク内の既定のルーティング テーブル エントリを変更できます。 ルーティング動作を制御すると、特定のデバイス、またはデバイスのグループから仮想ネットワークに出入りするすべてのトラフィックに、特定の場所を経由させる上で役立ちます。

たとえば、仮想ネットワークに仮想ネットワーク セキュリティ アプライアンスがあるとします。 仮想ネットワークとの間のすべてのトラフィックに仮想セキュリティ アプライアンスを経由させたいと考えています。 これは、Azure で[ユーザー定義ルート](../../virtual-network/virtual-networks-udr-overview.md) (UDR) を構成することで実現します。

[強制トンネリング](https://www.petri.com/azure-forced-tunneling)は、サービスがインターネット上のデバイスとの接続を開始する許可を得られないようにするメカニズムです。 これは、着信接続を受け入れて、それに応答することとは異なることにご注意ください。 フロントエンド Web サーバーは、インターネット ホストからの要求に応答することが必要です。したがって、インターネットからのトラフィックがこれらの Web サーバーに着信することは許可され、Web サーバーが応答することも許可されます。

許可すべきでないのは、フロントエンド Web サーバーが送信要求を開始することです。 そのような要求はセキュリティ リスクになり得ます。これらの接続を使用してマルウェアがダウンロードされる恐れがあるためです。 これらのフロントエンド サーバーがインターネットに向けて送信要求を開始する必要があるとしても、通常は、それらの要求にオンプレミスの Web プロキシを必ず経由させる必要があります。 これにより、URL フィルタリングとログ記録が可能になります。

別の方法として、強制トンネリングを使用してそれを避けることもできます。 強制トンネリングを有効にすると、インターネットに対するすべての接続はオンプレミス ゲートウェイを必ず経由します。 強制トンネリングは、UDR を活用して構成できます。

詳細情報:

* [ユーザー定義のルートおよび IP 転送とは](../../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>仮想ネットワークのセキュリティ アプライアンス

NSG、UDR、および強制トンネリングを使用すると、 [OSI モデル](https://en.wikipedia.org/wiki/OSI_model)のネットワーク層とトランスポート層に一定レベルのセキュリティ保護を確保することができますが、場合によっては、ネットワーク層より高いレベルのセキュリティが必要になることもあります。

たとえば、セキュリティ要件に次の事柄が含まれることがあります。

* アプリケーションに対するアクセスを許可する前の認証と承認
* 侵入検出と侵入応答
* アプリケーション層における上位プロトコルの検査
* URL フィルタリング
* ネットワーク レベルのウイルス対策とマルウェア対策
* アンチボット保護
* アプリケーション アクセス制御
* 追加の DDoS 保護 (Azure ファブリック自体によって提供される DDoS 保護に追加される保護)

Azure パートナー ソリューションを使用すれば、これらの拡張ネットワーク セキュリティ機能を利用できます。 最新の Azure パートナー ネットワーク セキュリティ ソリューションについては、[Azure Marketplace](https://azure.microsoft.com/marketplace/) にアクセスし、"security" および "network security" を検索すると見つかります。

## <a name="azure-firewall"></a>Azure Firewall

Azure Firewall は、Azure Virtual Network リソースを保護するクラウドベースのマネージド ネットワーク セキュリティ サービスです。 これは、組み込みの高可用性とクラウドによる無制限のスケーラビリティを備えた、完全にステートフルなサービスとしてのファイアウォールです。 機能の一部を以下に示します。

* 高可用性
* クラウドのスケーラビリティ
* アプリケーションの FQDN のフィルタリング規則
* ネットワーク トラフィックのフィルタリング規則

詳細情報:

* [Azure Firewall の概要](../../firewall/overview.md)

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>セキュリティで保護されたリモート アクセスとクロスプレミス接続

Azure リソースのセットアップ、構成、管理は、リモートで実行する必要があります。 また、オンプレミスと Azure パブリック クラウドの両方にコンポーネントがある [ハイブリッド IT](https://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) ソリューションをデプロイする場合もあります。 こうしたシナリオの場合、セキュリティで保護されたリモート アクセスが必要になります。

Azure のネットワークは、セキュリティで保護されたリモート アクセスの以下のシナリオをサポートしています。

* 個々のワークステーションから仮想ネットワークへの接続
* オンプレミス ネットワークから仮想ネットワークへの VPN による接続
* オンプレミス ネットワークから仮想ネットワークへの専用 WAN リンクによる接続
* 仮想ネットワークどうしの接続

### <a name="connect-individual-workstations-to-a-virtual-network"></a>個々のワークステーションから仮想ネットワークへの接続

Azure の仮想マシンとサービスの管理を個々の開発者や運用担当者に許可する必要がある場合があります。 たとえば、仮想ネットワーク上の仮想マシンにアクセスする必要があるとします。 しかし、セキュリティ ポリシーでは、RDP や SSH を使って個々 の仮想マシンにリモート アクセスすることが許可されていません。 この場合、[ポイント対サイト VPN](../../vpn-gateway/point-to-site-about.md) 接続を使用できます。

ポイント対サイト VPN 接続では、ユーザーと仮想ネットワークの間にセキュリティで保護されたプライベート接続を設定できます。 VPN 接続が確立されると、ユーザーは RDP や SSH を VPN リンク経由で使用して、仮想ネットワーク上の任意の仮想マシンにアクセスできるようになります (ユーザーが認証可能で、かつ承認済みである場合)。ポイント対サイト VPN は以下をサポートしています。

* Secure Socket トンネリング プロトコル (SSTP)。これは、SSL ベースの独自の VPN プロトコルです。 SSL VPN ソリューションはファイアウォールを通過できます。これは、ほとんどのファイアウォールで開かれている TCP ポート 443 が TLS/SSL で使用されるためです。 SSTP は、Windows デバイスでのみサポートされます。 Azure では、SSTP を備えたすべてのバージョンの Windows (Windows 7 以降) がサポートされています。

* IKEv2 VPN。これは、標準ベースの IPsec VPN ソリューションです。 IKEv2 VPN は、Mac デバイス (OSX バージョン 10.11 以上) から接続する際に使用できます。

* [OpenVPN](https://azure.microsoft.com/updates/openvpn-support-for-azure-vpn-gateways/)

詳細情報:

* [PowerShell を使用して仮想ネットワークへのポイント対サイト接続を構成する](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-vpn"></a>オンプレミス ネットワークから仮想ネットワークへの VPN による接続

組織によっては、企業ネットワークの全体 (またはその一部) を仮想ネットワークに接続するニーズが生じることがあります。 このようなニーズは、[組織のオンプレミス データセンターを Azure に拡張する](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84)、ハイブリッド IT のシナリオでよく生じます。 多くの場合は、サービスの一部を Azure でホストし、別の部分をオンプレミスでホストします。 この方法はたとえば、フロント エンドの Web サーバーが Azure 上にあり、バックエンドのデータベースがオンプレミス上にあるソリューションなどで使用されます。 このような "プレミス横断型" の接続を使用すると、Azure 上にあるリソースの管理もより安全に行えるようになり、Active Directory ドメイン コントローラーを Azure に拡張するシナリオなどにも対応可能になります。

この接続を実現する 1 つの方法は、 [サイト間 VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn)を使用することです。 サイト間 VPN とポイント間 VPN の違いは、後者の場合、 単一のデバイスが仮想ネットワークに接続されるという点です。 サイト間 VPN では、ネットワーク (オンプレミス ネットワークなど) の全体が仮想ネットワークに接続されます。 仮想ネットワークへのサイト間 VPN では、セキュリティに優れた IPsec トンネル モードの VPN プロトコルが使用されます。

詳細情報:

* [Azure Portal を使用してサイト間 VPN 接続を持つ Resource Manager VNet を作成する](../../vpn-gateway/tutorial-site-to-site-portal.md)
* [VPN Gateway について](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

### <a name="connect-your-on-premises-network-to-a-virtual-network-with-a-dedicated-wan-link"></a>オンプレミス ネットワークから仮想ネットワークへの専用 WAN リンクによる接続

ポイント対サイト VPN 接続とサイト間 VPN 接続は、クロスプレミス接続を有効にする上で効果的です。 しかし、これらの方法には次の欠点があると考える組織もあります。

* VPN 接続ではデータがインターネット上を移動します。 そのため、パブリック ネットワークを移動するデータに潜在的なセキュリティ問題が発生します。 また、インターネット接続については信頼性と可用性を保証することはできません。
* 仮想ネットワークへの VPN 接続では、最大発信速度が 200 Mbps 前後であるため、アプリケーションや目的によっては、十分な帯域幅が確保できません。

クロスプレミス接続で最高レベルのセキュリティと可用性を必要とする企業は、専用 WAN リンクを使用してリモート サイトに接続するのが一般的です。 Azure では、専用の WAN リンクを使用して、オンプレミスのネットワークを仮想ネットワークに接続できます。 Azure ExpressRoute、ExpressRoute Direct、および Express Route Global Reach によって、これが可能になっています。

詳細情報:

* [ExpressRoute の技術概要](../../expressroute/expressroute-introduction.md)
* [ExpressRoute Direct](../../expressroute/expressroute-erdirect-about.md)
* [ExpressRoute Global Reach](../../expressroute/expressroute-global-reach.md)

### <a name="connect-virtual-networks-to-each-other"></a>仮想ネットワークどうしの接続

組織のシステムには、多数の仮想ネットワークが使用されることがあります。 これにはさまざまな理由が考えられます。 管理を簡略化するためにそうすることもあれば、セキュリティの向上が目的の場合もあるでしょう。 リソースを複数の仮想ネットワークに配置する理由は何であれ、各ネットワーク上のリソースを相互に接続する必要が生じることは、よくあることです。

ある仮想ネットワーク上のサービスを別の仮想ネットワーク上のサービスに接続する 1 つの方法として、インターネットを介した "ループバック" という方法があります。 これは、1 つの仮想ネットワークで接続を開始し、インターネットを経由して、宛先の仮想ネットワークに戻すというものです。 ただし、この接続方法では、インターネット ベースの通信につきもののセキュリティ問題が発生します。

より望ましいのは、2 つの仮想ネットワーク間を接続する、サイト間 VPN を作成する方法でしょう。 この方法では、前述したプレミス横断型のサイト間 VPN 接続と同じ、[IPSec トンネル モード](/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10))のプロトコルが使用されます。

このアプローチの利点は、インターネット経由で接続するのではなく、Azure ネットワーク ファブリックを介して VPN 接続を確立できるという点です。 これによって、インターネット経由で接続するサイト間 VPN よりも高度なセキュリティが確保されます。

詳細情報:

* [Azure リソース マネージャーと PowerShell を使用した VNet 間の接続の構成](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

仮想ネットワークを接続する別の方法は [VNET ピアリング](../../virtual-network/virtual-network-peering-overview.md)です。 この機能を使用して、2 つの Azure ネットワーク間の通信が、インターネット経由なしで Microsoft バックボーン インフラストラクチャで発生するように、それらを接続できます。 VNET ピアリングは、同じリージョン内の 2 つの Vnet、または Azure リージョン間で 2 つの VNET を接続できます。 NSG を使用して、異なるサブネットまたはシステム間の接続を制限できます。

## <a name="availability"></a>可用性

可用性は、あらゆるセキュリティ プログラムの重要な構成要素です。 アクセスする必要のある対象にユーザーとシステムがネットワーク経由でアクセスできない場合、サービスが損なわれたと見なされる可能性があります。 Azure のネットワーク テクノロジは、次の高可用性メカニズムをサポートしています。

* HTTP ベースの負荷分散
* ネットワーク レベルの負荷分散
* グローバル負荷分散

負荷分散は、複数のデバイス間で接続を均等に配分するように設計されたメカニズムです。 負荷分散の目的は次のとおりです。

* 可用性を高める。 複数のデバイス間で接続の負荷を分散することで、1 つ以上のデバイスが使用できなくなった場合でも、サービスの中断を回避できます。 残りのオンライン デバイスで実行されているサービスから、コンテンツを提供することができます。
* パフォーマンスを向上させる。 複数のデバイス間で接続の負荷を分散することにより、1 つのデバイスですべての処理負荷を負う必要がなくなります。 その代わりに、コンテンツ提供に伴う処理とメモリの要求は、複数のデバイスに分散されます。

### <a name="http-based-load-balancing"></a>HTTP ベースの負荷分散

Web ベースのサービスを実行している組織では、それらの Web サービスの前面に、HTTP ベースのロード バランサーを配置することがよくあります。 これは、適切なレベルのパフォーマンスと高可用性を確保するためのものです。 通常、ネットワーク ベースのロード バランサーでは、ネットワーク層とトランスポート層のプロトコルが使用されますが、 HTTP ベースのロード バランサーでは、HTTP プロトコルの特性に基づいて分散方法が決定されます。

Azure では、Azure Application Gateway を使用することで、Web ベースのサービスに HTTP ベースの負荷分散を実装できます。 Application Gateway では、次の機能がサポートされます。

* Cookie ベースのセッション アフィニティ。 この機能では、ロード バランサーの背後にあるいずれかのサーバーに対して確立された接続が、クライアントとサーバーの間で正常に保たれていることを確認できます。 これにより、トランザクションの安定性が保証されます。
* TLS オフロード。 クライアントがロード バランサーに接続すると、そのセッションは HTTPS (TLS) プロトコルを使用して暗号化されます。 ただし、パフォーマンスを高めるために、ロード バランサーとその背後にある Web サーバーとの間の接続に、HTTP (暗号化されていない) プロトコルを使用することもできます。 これを "TLS オフロード" と呼びます。ロード バランサーの背後にある Web サーバーでは、暗号化のためのプロセッサ オーバーヘッドが発生しないため、 要求をより迅速に処理することができます。
* URL ベースのコンテンツ ルーティング。 この機能を使用すると、ロード バランサーは、接続の転送先をターゲット URL に基づいて決定できるようになります。 これにより、IP アドレスに基づいて負荷分散の決定を行うソリューションに比べて柔軟性がずっと高まります。

詳細情報:

* [Application Gateway の概要](../../application-gateway/overview.md)

### <a name="network-level-load-balancing"></a>ネットワーク レベルの負荷分散

HTTP ベースの負荷分散とは対照的に、ネットワーク レベルの負荷分散では、IP アドレスとポート (TCP または UDP) 番号に基づいて分散方法が決定されます。
Azure におけるネットワーク レベルの負荷分散の利点を活用するには、Azure Load Balancer を使用できます。 Load Balancer の主要な特性には、次の事柄が含まれます。

* IP アドレスとポート番号に基づくネットワーク レベルの負荷分散。
* あらゆるアプリケーション層プロトコルのサポート。
* Azure 仮想マシンとクラウド サービスのロール インスタンスへの負荷分散。
* インターネットに接続する (外部負荷分散) アプリケーションと仮想マシン、およびインターネットに接続しない (内部負荷分散) アプリケーションと仮想マシンのどちらにも使用可能。
* エンドポイント監視 (ロード バランサーの背後にあるサービスが使用不可になっていないか判別するために使用します)。

詳細情報:

* [複数の仮想マシンまたはサービス間でインターネットに接続するロード バランサー](../../load-balancer/load-balancer-overview.md)
* [内部ロード バランサーの概要](../../load-balancer/load-balancer-overview.md)

### <a name="global-load-balancing"></a>グローバル負荷分散

可能な限り最高レベルの可用性が必要な組織もあります。 この目的を実現する 1 つの方法は、グローバルに分散したデータセンターでアプリケーションをホストすることです。 世界中にあるデータセンターでアプリケーションがホストされていれば、1 つの地域全体が使用不可になっても、アプリケーションは引き続き稼働できます。

この負荷分散方法では、パフォーマンス上のメリットも得られます。 サービスへの要求を、要求元のデバイスに最も近いデータ センターに転送できるからです。

Azure では、Azure Traffic Manager を使用してグローバル負荷分散の利点が得られます。

詳細情報:

* [Traffic Manager について](../../traffic-manager/traffic-manager-overview.md)

## <a name="name-resolution"></a>名前解決

名前解決は、Azure でホストするすべてのサービスにとって重要な機能です。 セキュリティ上の観点からすると、名前解決機能が危害を受けると、攻撃者によって、要求がユーザー サイトから攻撃者のサイトにリダイレクトされる可能性があります。 名前解決がセキュリティで保護されていることは、クラウドでホストされているすべてのサービスにとって必要条件となっています。

対処する必要がある名前解決は、次の 2 種類です。

* 内部名前解決。 これは、仮想ネットワーク、オンプレミス ネットワーク、またはそれら両方のサービスによって使用されます。 内部名前解決に使用される名前は、インターネットからはアクセスできません。 最適なセキュリティを確保するため、内部名前解決スキームは外部ユーザーからアクセスできないことが重要です。
* 外部名前解決。 これは、オンプレミス ネットワークや仮想ネットワークの外部のユーザーやデバイスによって使用されます。 これらの名前は、インターネットから参照することができ、クラウド ベースのサービスに接続するために使用されます。

内部名前解決には、次の 2 つのオプションがあります。

* 仮想ネットワークの DNS サーバー。 新しい仮想ネットワークを作成すると、DNS サーバーが作成されます。 この DNS サーバーは、その仮想ネットワークにあるマシンの名前を解決できます。 この DNS サーバーは Azure ファブリック マネージャーによって管理され、お客様は構成を変更することができません。これにより、安全な名前解決ソリューションが提供されます。
* 独自の DNS サーバーの導入。 お客様が選んだ独自の DNS サーバーを、仮想ネットワーク上に配置することもできます。 この DNS サーバーは、Active Directory 統合 DNS サーバーでも、Azure パートナーが提供する専用 DNS サーバー ソリューション (Azure Marketplace から入手可能) でも構いません。

詳細情報:

* [Virtual Network の概要](../../virtual-network/virtual-networks-overview.md)
* [仮想ネットワークで使用される DNS サーバーの管理](../../virtual-network/manage-virtual-network.md#change-dns-servers)

外部名前解決には、次の 2 つのオプションがあります。

* 独自の外部 DNS サーバーをオンプレミスでホストする。
* 独自の外部 DNS サーバーをサービス プロバイダーによってホストする。

多くの大規模な組織は独自の DNS サーバーをオンプレミスでホストします。 それは、そのためのネットワークの専門知識とグローバルな人的/物的リソースが存在しているためです。

ほとんどの場合には、サービス プロバイダーを使用して独自の DNS 名前解決サービスをホストする方が優れています。 こうしたサービス プロバイダーには、名前解決サービスの非常に高い可用性を確保できるネットワークの専門知識とグローバルな人的/物的リソースがあります。 DNS サービスで可用性は不可欠な要素です。名前解決サービスで障害が発生すると、インターネットに接続するサービスに誰もアクセスできなくなるからです。

Azure は、Azure DNS という形で可用性とパフォーマンスの高い外部 DNS ソリューションを提供します。 この外部名前解決ソリューションは、世界規模の Azure DNS インフラストラクチャを活用します。 これによって、他の Azure サービスと同じ資格情報、API、ツール、課金情報を使用して、Azure でドメインをホストできます。 また Azure の一部として、プラットフォームに組み込まれている強力なセキュリティ制御機能も継承します。

詳細情報:

* [Azure DNS の概要](../../dns/dns-overview.md)
* [Azure DNS プライベート ゾーン](../../dns/private-dns-overview.md)を使用して、カスタム DNS ソリューションの追加なしで名前を自動的に割り当てる代わりに、Azure リソースのプライベート DNS 名を構成できます。

## <a name="perimeter-network-architecture"></a>境界ネットワーク アーキテクチャ

多くの大規模組織では、境界ネットワークを使用してネットワークをセグメント化し、インターネットとサービスの間に緩衝地帯を設けます。 ネットワークの境界部分はセキュリティの低いゾーンと見なされ、高価値の資産がそのネットワーク セグメントに配置されることはありません。 通常、ネットワーク セキュリティ デバイスには、境界ネットワーク セグメント上のネットワーク インターフェイスが使用されます。 インターネットからの着信接続を受け付ける仮想マシンやサービスがあるネットワークには、別のネットワーク インターフェイスが接続されます。

境界ネットワークの設計方法にはさまざまなものがあります。 境界ネットワークをデプロイするかどうかや、どのような種類の境界ネットワークを使用するかは、ネットワークのセキュリティ要件によって決定されます。

詳細情報:

* [Microsoft クラウド サービスとネットワーク セキュリティ](network-best-practices.md)

## <a name="azure-ddos-protection"></a>Azure の DDoS 保護

分散型サービス拒否 (DDoS) 攻撃は、アプリケーションをクラウドに移行している顧客が直面する可用性とセキュリティに関する最大の関心事の一部です。 DDoS 攻撃では、アプリケーションのリソースを使い果たし、正当なユーザーがアプリケーションを使用できなくなるようにすることが試みられます。 DDoS 攻撃は、インターネット経由で一般に到達可能なすべてのエンドポイントで実行できます。
Microsoft では、Azure Platform の一部として、**Basic** と呼ばれる DDoS Protection を提供しています。 これは無料で提供され、一般的なネットワーク レベル攻撃の監視とリアルタイムの対応策に常に含まれています。 DDoS Protection **Basic** に含まれている保護に加え、**Standard** オプションを有効にできます。 DDoS Protection Standard の機能には、次のものが含まれます。

* **ネイティブのプラットフォーム統合:** Azure にネイティブに統合します。 Azure Portal による構成が含まれます。 DDoS Protection Standard は、ユーザーのリソースおよびリソース構成を理解しています。
* **ターンキー保護:** DDoS Protection Standard が有効になるとすぐに、簡略化された構成によって、仮想ネットワーク上のすべてのリソースが直ちに保護されます。 ユーザーが介入したり、ユーザーが定義したりする必要はありません。 DDoS Protection Standard では、攻撃が検出されるとすぐに自動で軽減されます。
* **常時接続のトラフィック監視:** DDoS 攻撃の兆候を検出するために、アプリケーションのトラフィック パターンが 24 時間 365 日監視されます。 軽減策は、保護ポリシーを超えた場合に実行されます。
* **攻撃の軽減策レポート:** 攻撃の軽減策レポートでは、集約されたネットワーク フロー データを使用して、リソースを対象とした攻撃に関する詳細な情報を提供します。
* **攻撃の軽減フロー ログ:** 攻撃の軽減フロー ログを使用すると、アクティブな DDoS 攻撃中に、破棄されたトラフィック、転送されたトラフィック、およびその他の攻撃データをほぼリアルタイムに確認できます。
* **アダプティブ チューニング:** インテリジェント トラフィック プロファイリングにより、一定期間にわたってアプリケーションのトラフィックが学習され、そのサービスに最も適したプロファイルが選択および更新されます。 このプロファイルは、時間の経過とともにトラフィックが変化すると調整されます。 レイヤー 3 からレイヤー 7 までの保護:Web アプリケーション ファイアウォールと併用すると、全スタックにわたって DDoS 保護が提供されます。
* **広範囲にわたる軽減スケール:** 60 種類を超える攻撃を軽減することができ、地球規模の容量を利用して、過去最大の DDoS 攻撃からも保護されます。
* **攻撃メトリック:** 各攻撃から要約されたメトリックに Azure Monitor 経由でアクセスできます。
* **攻撃アラート:** 組み込みの攻撃メトリックを使用して、攻撃の開始時と停止時、およびその攻撃の期間にわたってアラートを構成できます。 アラートは、Microsoft Azure Monitor ログ、Splunk、Azure Storage、電子メール、Azure portal などの運用ソフトウェアに統合されます。
* **コストの保証:** データ転送とアプリケーションは、ドキュメント化された DDoS 攻撃のサービス クレジットをスケールアウトします。
* **DDoS への迅速な対応:** DDoS Protection Standard のお客様は、アクティブな攻撃中に Rapid Response チームを利用できるようになりました。 DRR では、攻撃の調査、攻撃中の独自の軽減策、攻撃後の分析に関して支援できます。


詳細情報:

* [DDOS 保護の概要](../../ddos-protection/ddos-protection-overview.md)

## <a name="azure-front-door"></a>Azure Front Door

Azure Front Door サービスでは、Web トラフィックのグローバルなルーティングを定義、管理、監視することができます。 最適なパフォーマンスと高可用性を得るため、トラフィックのルーティングが最適化されます。 Azure Front Door を使用すると、アクセス制御のためのカスタムの Web アプリケーション ファイアウォール (WAF) 規則を作成して、クライアント IP アドレス、国番号、および http パラメーターに基づいて HTTP/HTTPS ワークロードを不正使用から保護することができます。 さらに、Front Door では悪意のあるボット トラフィックと戦うためのレート制限ルールを作成することもできます。これには、TLS オフロードと、HTTP/HTTPS 要求によるアプリケーション レイヤーの処理が含まれます。

Front Door プラットフォーム自体は、Azure DDoS Protection Basic によって保護されます。 さらに保護するために、Azure DDoS Protection Standard を VNET で有効にし、自動チューニングとリスク軽減によってネットワーク層 (TCP または UDP) 攻撃からリソースを保護することもできます。 Front Door はレイヤー 7 のリバース プロキシであり、既定では Web トラフィックに、バックエンド サーバーへのパススルーのみを許可し、他の種類のトラフィックはブロックします。

詳細情報:

* Azure Front Door 機能セット全体の詳細については、[Azure Front Door の概要](../../frontdoor/front-door-overview.md)に関する記事で確認できます

## <a name="azure-traffic-manager"></a>Azure Traffic Manager

Azure Traffic Manager は、世界中の Azure リージョン間でサービスへのトラフィックを最適に配分しつつ、高可用性と応答性を実現する DNS ベースのトラフィック ロード バランサーです。 Traffic Manager は、DNS を使用し、トラフィック ルーティング方法とエンドポイントの正常性に基づいて最適なサービス エンドポイントにクライアント要求を誘導します。 エンドポイントは、Azure の内部または外部でホストされている、インターネットに公開されたサービスです。 Traffic Manager はエンドポイントを監視し、使用できないエンドポイントにはトラフィックを転送しません。

詳細情報:

* [Azure Traffic Manager の概要](../../traffic-manager/traffic-manager-overview.md)

## <a name="monitoring-and-threat-detection"></a>監視と脅威の検出

Azure では、この重要な分野で、早期検出、監視、ネットワーク トラフィックの収集および確認に役立つ機能を提供しています。

### <a name="azure-network-watcher"></a>Azure Network Watcher

Azure Network Watcher では、セキュリティ問題を特定するためのまったく新しいツール セットを使用して、問題のトラブルシューティングを行うことができます。

[セキュリティ グループ ビュー](../../network-watcher/network-watcher-security-group-view-overview.md)では、Virtual Machines の監査要件やセキュリティ要件に対するコンプライアンス状況を確認できます。 この機能を使用すれば、組織で定義されたベースライン ポリシーを、各 VM に対する効果的なルールと比較しながら、プログラマティックな監査を実行できます。 構成のずれを特定するときにこの機能が役立ちます。

[パケット キャプチャ](../../network-watcher/network-watcher-packet-capture-overview.md)を使用すると、仮想マシンとの間のネットワーク トラフィックをキャプチャすることができます。 お客様は、ネットワーク統計情報を収集し、アプリケーションの問題をトラブルシューティングできます。これは、ネットワーク侵入を調査するうえで非常に効果的です。 この機能を Azure Functions と一緒に使用することで、Azure の特定のアラートに反応してネットワーク キャプチャを開始することができます。

Network Watcher と、ラボで機能の一部のテストを開始する方法について詳しくは、[Azure Network Watcher の監視の概要](../../network-watcher/network-watcher-monitoring-overview.md)に関するページをご覧ください。

> [!NOTE]
> このサービスの可用性とステータスに関する最新の通知については、[Azure の更新情報](https://azure.microsoft.com/updates/?product=network-watcher)に関するページをご覧ください。

### <a name="azure-security-center"></a>Azure Security Center

Azure Security Center は、脅威の回避、検出、対応に役立つサービスで、Azure リソースのセキュリティを高度に視覚化して制御できます。 これにより、Azure サブスクリプション全体に統合セキュリティの監視とポリシーの管理を提供し、気付かない可能性がある脅威を検出し、多数のセキュリティ ソリューションと連動します。

Security Center は、ネットワーク セキュリティの最適化と監視に役立つ次の機能を備えています。

* ネットワーク セキュリティに関する推奨事項を提供する。
* ネットワーク セキュリティ構成の状態を監視する。
* ネットワーク ベースの脅威をエンドポイント レベルとネットワーク レベルの両方で警告する。

詳細情報:

* [Azure Security Center 入門](../../security-center/security-center-introduction.md)

### <a name="virtual-network-tap"></a>仮想ネットワーク TAP

Azure 仮想ネットワーク TAP (ターミナル アクセス ポイント) を使用すると、仮想マシン ネットワークのトラフィックをネットワーク パケット コレクターまたは分析ツールに連続してストリーミングできます。 コレクターまたは分析ツールは、ネットワーク仮想アプライアンス パートナーから提供されています。 同じ仮想ネットワーク TAP リソースを使用して、同じサブスクリプションまたは異なるサブスクリプションの複数のネットワーク インターフェイスからのトラフィックを集計できます。

詳細情報:

* [仮想ネットワーク TAP](../../virtual-network/virtual-network-tap-overview.md)

### <a name="logging"></a>ログ記録

ネットワーク レベルにおけるログ記録は、あらゆるネットワーク セキュリティ シナリオの主要な機能です。 Azure では、NSG に関して入手した情報をログに記録して、ネットワーク レベルのログ情報を取得できます。 NSG ログ記録によって、次のログから情報を取得します。

* [アクティビティ ログ](../../azure-monitor/essentials/platform-logs-overview.md)。 これらのログは、Azure サブスクリプションに送信されたすべての操作を表示するときに使用します。 このログは既定で有効になっており、Azure ポータルで使用できます。 以前は監査ログや操作ログと呼ばれていました。
* イベント ログ。 これらのログは、適用された NSG ルールについての情報を提供します。
* カウンター ログ。 このログを使用すると、トラフィックを拒否または許可するために各 NSG ルールが適用された回数が分かります。

優れたデータ視覚化ツールである [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/)を使用して、これらのログを表示および分析することもできます。
詳細情報:

* [ネットワーク セキュリティ グループ (NSG) の Azure Monitor ログ](../../virtual-network/virtual-network-nsg-manage-log.md)