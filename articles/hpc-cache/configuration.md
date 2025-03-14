---
title: Azure HPC Cache 設定の構成
description: キャッシュに MTU や no-root-squash などの追加設定を構成する方法、および Azure Blob Storage ターゲットから高速スナップショットにアクセスする方法について説明します。
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 12/21/2020
ms.author: v-erkel
ms.openlocfilehash: 02bf862cdc3b20ef3e5fdb024f474267efa0c70d
ms.sourcegitcommit: 6cca6698e98e61c1eea2afea681442bd306487a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/24/2020
ms.locfileid: "97760505"
---
# <a name="configure-additional-azure-hpc-cache-settings"></a>Azure HPC Cache の追加設定を構成する

Azure portal の **[構成]** ページには、いくつかの設定をカスタマイズするためのオプションがあります。 ほとんどのユーザーは、これらの設定を既定値から変更する必要はありません。

この記事では、Azure Blob Storage ターゲットに対してスナップショット機能を使用する方法についても説明します。 スナップショット機能には、構成可能な設定はありません。

設定を表示するには、Azure portal でキャッシュの **[構成]** ページを開きます。

![Azure portal の [構成] ページのスクリーンショット](media/configuration.png)

> [!TIP]
> [Azure HPC Cache の管理に関するビデオ](https://azure.microsoft.com/resources/videos/managing-hpc-cache/)には、構成ページとその設定が表示されます。

## <a name="adjust-mtu-value"></a>MTU 値を調整する
<!-- linked from troubleshoot-nas article -->

**[MTU サイズ]** のラベルが付いたドロップダウン メニューを使用して、キャッシュの最大転送ユニット サイズを選択できます。

既定値は 1,500 バイトですが、1,400 に変更できます。

> [!NOTE]
> キャッシュの MTU サイズを小さくする場合は、キャッシュと通信するクライアントと記憶域システムの MTU 設定が同じか、小さい値になっていることを確認します。

キャッシュの MTU 値を小さくすると、キャッシュのネットワークの残りの部分でパケット サイズの制限を回避するのに役立ちます。 たとえば、一部の VPN では、フルサイズの 1,500 バイトのパケットを正常に転送できません。 VPN 経由で送信されるパケットのサイズを小さくすると、その問題が解消される可能性があります。 ただし、キャッシュの MTU 設定を下げると、そのキャッシュと通信するその他のコンポーネント (クライアントやストレージ システムなど) の MTU 設定も下げて、通信の問題を回避する必要があることにご注意ください。

他のシステム コンポーネントの MTU 設定を変更したくない場合は、キャッシュの MTU 設定を下げないでください。 VPN パケット サイズの制限を回避するための解決策は他にもあります。 この問題の診断と解決に関する詳細については、NAS のトラブルシューティング記事の「[VPN パケット サイズの制限を調整する](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions)」を参照してください。

Azure 仮想ネットワークの MTU 設定の詳細については、「[Azure VM の TCP/IP パフォーマンス チューニング](../virtual-network/virtual-network-tcpip-performance-tuning.md)」をご覧ください。

## <a name="configure-root-squash"></a>root squash を構成する
<!-- linked from troubleshoot and from access policies -->

**[ルート スカッシュを有効にする]** の設定では、クライアント コンピューター上のルート ユーザーからの要求が Azure HPC Cache によってどのように処理されるかが制御されます。

ルート スカッシュを有効にすると、クライアントのルート ユーザーは、Azure HPC Cache を介して要求を送信すると、自動的にユーザー "nobody" にマップされます。 また、クライアント要求で set-UID アクセス許可ビットを使用できなくなります。

ルート スカッシュが無効になっている場合は、クライアント ルート ユーザー (UID 0) からの要求は、バックエンド NFS ストレージ システムにルートとして渡されます。 この構成では、不適切なファイル アクセスが許可される可能性があります。

キャッシュでルート スカッシュを設定すると、ストレージ ターゲットとして使用される NAS システムで必要な ``no_root_squash`` の設定を補うのに役立ちます。 (詳細については [NFS ストレージ ターゲットの前提条件](hpc-cache-prerequisites.md#nfs-storage-requirements)に関するセクションを参照してください)。また、Azure Blob Storage ターゲットと共に使用すると、セキュリティを向上させることもできます。

既定の設定は **[はい]** です (2020 年 4 月より前に作成されたキャッシュでは、既定の設定が **[いいえ]** になっている場合があります)。

> [!TIP]
> [クライアント アクセス ポリシー](access-policies.md#root-squash)をカスタマイズすることによって、特定のストレージのエクスポートに対してルート スカッシュを設定することもできます。

## <a name="view-snapshots-for-blob-storage-targets"></a>BLOB ストレージ ターゲットのスナップショットを表示する

Azure HPC Cache では、Azure Blob Storage ターゲットのストレージ スナップショットが自動的に保存されます。 スナップショットは、バックエンド ストレージ コンテナーのコンテンツに対してクイック リファレンス ポイントを提供します。

スナップショットは、データ バックアップに代わるものではなく、キャッシュ データの状態に関する情報は含まれていません。

> [!NOTE]
> このスナップショット機能は、NetApp または Isilon ストレージ ソフトウェアに含まれているスナップショット機能とは異なります。 これらのスナップショットの実装によって、スナップショットを取得する前に、キャッシュからバックエンド ストレージ システムに変更がフラッシュされます。
>
> 効率を高めるために、Azure HPC Cache スナップショットでは、最初に変更をフラッシュせず、BLOB コンテナーに書き込まれたデータのみが記録されます。 このスナップショットはキャッシュ データの状態を表していないので、最近の変更が含まれていない可能性があります。

この機能は、Azure Blob Storage ターゲットでのみ使用でき、その構成を変更することはできません。

スナップショットは 8 時間ごと (UTC 0:00、08:00、16:00) に取得されます。

Azure HPC Cache では、毎日、毎週、毎月のスナップショットが、新しいスナップショットに置き換えられるまで保存されます。 次の制限があります。

* 毎日のスナップショットは最大 20 個
* 毎週のスナップショットは最大 8 個
* 毎月のスナップショットは最大 3 個

スナップショットには、BLOB ストレージ ターゲットの名前空間の `.snapshot` ディレクトリからアクセスします。
