---
title: Azure Backup を使用して Azure に SAP HANA データベースをバックアップする
description: この記事では、Azure Backup サービスを使用して SAP HANA データベースを Azure 仮想マシンにバックアップする方法について説明します。
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: e7735c4240529cc6fc9bb6470934dd335d22aa77
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101719612"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Azure VM での SAP HANA データベースのバックアップ

SAP HANA データベースは、低い回復ポイントの目標値 (RPO) と長期リテンション期間を必要とする重要なワークロードです。 [Azure Backup](backup-overview.md) を使用して、Azure 仮想マシン (VM) 上で稼働している SAP HANA データベースをバックアップできます。

この記事では、Azure VM 上で稼働している SAP HANA データベースを Azure Backup Recovery Services コンテナーにバックアップする方法について説明します。

この記事では、次の方法について学習します。
> [!div class="checklist"]
>
> * コンテナーを作成して構成する
> * データベースを検出する
> * バックアップを構成する
> * オンデマンド バックアップ ジョブを実行する

>[!NOTE]
>2020 年 8 月 1 日の時点で、RHEL (7.4、7.6、7.7、8.1) 用の SAP HANA バックアップは一般提供されています。

>[!NOTE]
>**Azure VM での SQL Server の論理的な削除と Azure VM ワークロードでの SAP HANA の論理的な削除** が、プレビューで利用できるようになりました。<br>
>プレビューにサインアップするには、[AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) 宛てにご連絡ください。

## <a name="prerequisites"></a>前提条件

バックアップ用にデータベースを設定するには、「[前提条件](tutorial-backup-sap-hana-db.md#prerequisites)」と「[事前登録スクリプトで実行される処理](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)」のセクションを参照してください。

### <a name="establish-network-connectivity"></a>ネットワーク接続を確立する

すべての操作において、Azure VM で実行されている SAP HANA データベースには Azure Backup サービス、Azure Storage、Azure Active Directory への接続が必要です。 これは、プライベート エンドポイントを使用するか、必要なパブリック IP アドレスまたは FQDN へのアクセスを許可することによって実現できます。 必要な Azure サービスへの適切な接続を許可しないと、データベースの検出、バックアップの構成、バックアップの実行、データの復元などの操作の失敗につながる可能性があります。

次の表に、接続の確立に使用できるさまざまな選択肢を示します。

| **オプション**                        | **長所**                                               | **短所**                                            |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| プライベート エンドポイント                 | 仮想ネットワーク内のプライベート IP 経由でのバックアップを可能にする  <br><br>   ネットワークとコンテナーの側で詳細な制御を提供する | 標準のプライベート エンドポイント [コスト](https://azure.microsoft.com/pricing/details/private-link/)が発生する |
| NSG サービス タグ                  | 範囲の変更が自動的にマージされるため管理しやすい   <br><br>   追加のコストが発生しない | NSG でのみ使用可能  <br><br>    サービス全体へのアクセスを提供する |
| Azure Firewall の FQDN タグ          | 必要な FQDN が自動的に管理されるため管理しやすい | Azure Firewall でのみ使用可能                         |
| サービスの FQDN/IP へのアクセスを許可する | 追加のコストが発生しない   <br><br>  すべてのネットワーク セキュリティ アプライアンスとファイアウォールで動作する | 広範な IP または FQDN のセットへのアクセスが必要になる場合がある   |
| HTTP プロキシを使用する                 | VM に対するインターネット アクセスを単一の場所で実現                       | プロキシ ソフトウェアで VM を実行するための追加のコストが発生する         |

これらのオプションを使用する方法の詳細については、以下を参照してください。

#### <a name="private-endpoints"></a>プライベート エンドポイント

プライベート エンドポイントを使用すると、仮想ネットワーク内のサーバーから Recovery Services コンテナーに安全に接続できます。 プライベート エンドポイントでは、お使いのコンテナーの VNET アドレス空間からの IP アドレスが使用されます。 仮想ネットワーク内のリソースとコンテナー間のネットワーク トラフィックは、仮想ネットワークと Microsoft のバックボーン ネットワーク上のプライベート リンクを経由します。 これにより、パブリック インターネットへの露出が排除されます。 [こちら](./private-endpoints.md)から、Azure Backup のプライベート エンドポイントの詳細を参照してください。

#### <a name="nsg-tags"></a>NSG タグ

ネットワーク セキュリティ グループ (NSG) を使用する場合は、*AzureBackup* サービス タグを使用して、Azure Backup への発信アクセスを許可します。 Azure Backup タグに加えて、Azure AD (*AzureActiveDirectory*) および Azure Storage (*Storage*) に対して同様の [NSG 規則](../virtual-network/network-security-groups-overview.md#service-tags)を作成することによって、認証とデータ転送のための接続を許可する必要もあります。  次の手順では、Azure Backup タグの規則を作成するプロセスについて説明します。

1. **[すべてのサービス]** で、 **[ネットワーク セキュリティ グループ]** に移動して、ネットワーク セキュリティ グループを選択します。

1. **[設定]** で **[送信セキュリティ規則]** を選択します。

1. **[追加]** を選択します。 [セキュリティ規則の設定](../virtual-network/manage-network-security-group.md#security-rule-settings)の説明に従って、新しい規則を作成するために必要なすべての詳細を入力します。 オプション **[宛先]** が *[サービス タグ]* に、 **[宛先サービス タグ]** が *[AzureBackup]* に設定されていることを確認します。

1. **[追加]** を選択して、新しく作成した送信セキュリティ規則を保存します。

Azure Storage と Azure AD に対する NSG 送信セキュリティ規則も、同様に作成できます。 サービス タグの詳細については、[こちらの記事](../virtual-network/service-tags-overview.md)を参照してください。

#### <a name="azure-firewall-tags"></a>Azure Firewall タグ

Azure Firewall を使用している場合は、*AzureBackup* [Azure Firewall FQDN タグ](../firewall/fqdn-tags.md)を使用してアプリケーション規則を作成します。 これにより、Azure Backup へのすべての発信アクセスが許可されます。

#### <a name="allow-access-to-service-ip-ranges"></a>サービスの IP 範囲へのアクセスを許可する

サービスの IP へのアクセスを許可することを選択した場合は、[こちら](https://www.microsoft.com/download/confirmation.aspx?id=56519)から利用可能な IP 範囲の JSON ファイルを参照してください。 Azure Backup、Azure Storage、および Azure Active Directory に対応する IP へのアクセスを許可する必要があります。

#### <a name="allow-access-to-service-fqdns"></a>サービスの FQDN へのアクセスを許可する

次の FQDN を使用することで、サーバーから必要なサービスへのアクセスを許可することもできます。

| サービス    | アクセスするドメイン名                             |
| -------------- | ------------------------------------------------------------ |
| Azure Backup  | `*.backup.windowsazure.com`                             |
| Azure Storage | `*.blob.core.windows.net` <br><br> `*.queue.core.windows.net` |
| Azure AD      | [この記事](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online)に従って、セクション 56 および 59 の FQDN へのアクセスを許可します |

#### <a name="use-an-http-proxy-server-to-route-traffic"></a>トラフィックをルーティングするために HTTP プロキシ サーバーを使用する

Azure VM で実行されている SAP HANA データベースをバックアップする場合、VM 上のバックアップ拡張機能によって HTTPS API が使用され、管理コマンドが Azure Backup に送信されてデータが Azure Storage に送信されます。 また、バックアップ拡張機能では、認証に Azure AD を使用します。 HTTP プロキシ経由でこれらの 3 つのサービスのバックアップ拡張機能のトラフィックをルーティングします。 必要なサービスへのアクセスを許可するには、上記で説明した IP と FQDN の一覧を使用します。 認証済みプロキシ サーバーはサポートされません。

> [!NOTE]
> サービス レベルのプロキシはサポートされていません。 つまり、少数または選択されたサービス (Azure Backup サービス) からのプロキシ経由のトラフィックはサポートされていません。 データ全体またはトラフィック全体をプロキシによってルーティングするか、しないことはできます。

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>データベースを検出する

1. コンテナーの **[作業の開始]** で、 **[バックアップ]** を選択します。 **[ワークロードはどこで実行されていますか?]** で、 **[Azure VM の SAP HANA]** を選択します。
2. **[検出の開始]** を選択します。 これで、コンテナー リージョン内の保護されていない Linux VM の検出が開始されます。

   * 検出後、保護されていない VM は、ポータルで名前およびリソース グループ別に一覧表示されます。
   * VM が予期したとおりに一覧表示されない場合は、それが既にコンテナーにバックアップされているかどうかを確認してください。
   * 複数の VM を同じ名前にすることはできますが、それらは異なるリソース グループに属しています。

3. **[仮想マシンの選択]** で、リンクをクリックして、データベースの検出のために SAP HANA VM へのアクセス許可を Azure Backup サービスに与えるスクリプトをダウンロードします。
4. バックアップする SAP HANA データベースをホストしている各 VM でスクリプトを実行します。
5. VM でスクリプトを実行した後、 **[仮想マシンの選択]** で、VM を選択します。 次に、 **[データベースを検出]** を選択します。
6. Azure Backup によって、VM 上のすべての SAP HANA データベースが検出されます。 検出中に、Azure Backup によって VM がコンテナーに登録され、VM に拡張機能がインストールされます。 エージェントはデータベースにインストールされません。

    ![SAP HANA データベースを検出する](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>バックアップの構成  

ここでバックアップを有効にします。

1. 手順 2 で、 **[バックアップの構成]** を選択します。

    ![バックアップの構成](./media/backup-azure-sap-hana-database/configure-backup.png)
2. **[バックアップする項目の選択]** で、保護するデータベースをすべて選択し、 **[OK]** を選びます。

    ![バックアップする項目を選択する](./media/backup-azure-sap-hana-database/select-items.png)
3. **[バックアップ ポリシー]**  >  **[バックアップ ポリシーの選択]** で、以下の手順に従って、データベースの新しいバックアップ ポリシーを作成します。

    ![バックアップ ポリシーを選択する](./media/backup-azure-sap-hana-database/backup-policy.png)
4. ポリシーを作成した後、 **[バックアップ] メニュー** の **[バックアップの有効化]** を選択します。

    ![バックアップの有効化](./media/backup-azure-sap-hana-database/enable-backup.png)
5. ポータルの **[通知]** 領域で、バックアップ構成の進行状況を追跡します。

### <a name="create-a-backup-policy"></a>バックアップ ポリシーの作成

バックアップ ポリシーでは、バックアップが取得されるタイミングと、それらが保持される期間を定義します。

* ポリシーはコンテナー レベルで作成されます。
* 複数のコンテナーでは同じバックアップ ポリシーを使用できますが、各コンテナーにバックアップ ポリシーを適用する必要があります。

>[!NOTE]
>Azure Backup では、Azure VM で実行されている SAP HANA データベースをバックアップしている場合、夏時間変更に合わせた自動調整は行われません。
>
>必要に応じて手動でポリシーを変更してください。

次のように、ポリシー設定を指定します。

1. **[ポリシー名]** に新しいポリシーの名前を入力します。

   ![ポリシー名を入力する](./media/backup-azure-sap-hana-database/policy-name.png)
1. **完全バックアップのポリシー** で **[バックアップ頻度]** を選択し、 **[毎日]** または **[毎週]** を選びます。
   * **日次**:バックアップ ジョブが開始される、時刻とタイム ゾーンを選択します。
       * 完全バックアップを実行する必要があります。 このオプションをオフにすることはできません。
       * **[完全バックアップ]** を選択し、ポリシーを表示します。
       * 日次の完全バックアップを選択する場合は、差分バックアップを作成できません。
   * **毎週**:バックアップ ジョブが実行される、曜日、時刻、およびタイム ゾーンを選択します。

   ![バックアップの頻度を選択する](./media/backup-azure-sap-hana-database/backup-frequency.png)

1. **[リテンション期間]** で、完全バックアップのリテンション期間の設定を構成します。
    * 既定では、すべてのオプションが選択されています。 使用しないリテンション期間の制限をすべてクリアして、使用するものを設定します。
    * あらゆる種類のバックアップ (完全、差分、ログ) の最小保持期間は 7 日間です。
    * 復旧ポイントは、そのリテンション期間の範囲に基づいて、リテンション期間に対してタグ付けされます。 たとえば、日次での完全バックアップを選択した場合、日ごとにトリガーされる完全バックアップは 1 回だけです。
    * 特定の曜日のバックアップがタグ付けされ、週次でのリテンション期間と設定に基づいて保持されます。
    * 月次および年次のリテンション期間の範囲でも、同様の動作になります。

1. **完全バックアップのポリシー** メニューで、 **[OK]** を選択して設定を確定します。
1. **[差分バックアップ]** を選択して、差分ポリシーを追加します。
1. **差分バックアップのポリシー** で、 **[有効]** を選択して頻度とリテンション期間の制御を開きます。
    * 最多で、1 日に 1 回の差分バックアップをトリガーできます。
    * 差分バックアップは、最大 180 日間保持できます。 より長いリテンション期間が必要な場合は、完全バックアップを使用する必要があります。

    ![差分バックアップ ポリシー](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > 毎日のバックアップとしては、差分バックアップまたは増分バックアップのどちらかを選択できます。両方を選択することはできません。
1. **[増分バックアップ ポリシー]** で、 **[有効]** を選択して頻度と保有期間の制御を開きます。
    * 最多で、1 日に 1 回の増分バックアップをトリガーできます。
    * 増分バックアップは、最大 180 日間保持できます。 より長いリテンション期間が必要な場合は、完全バックアップを使用する必要があります。

    ![増分バックアップ ポリシー](./media/backup-azure-sap-hana-database/incremental-backup-policy.png)

1. **[OK]** を選択してポリシーを保存し、 **[バックアップ ポリシー]** のメイン メニューに戻ります。
1. **[ログ バックアップ]** を選択し、トランザクション ログ バックアップ ポリシーを追加します。
    * **[ログ バックアップ]** で、 **[有効化]** を選択します。  すべてのログ バックアップは SAP HANA で管理されるため、これを無効にすることはできません。
    * 頻度とリテンション期間の制御を設定します。

    > [!NOTE]
    > ログ バックアップでは、完全バックアップが正常に完了した後にのみ、フローが開始されます。

1. **[OK]** を選択してポリシーを保存し、 **[バックアップ ポリシー]** のメイン メニューに戻ります。
1. バックアップ ポリシーの定義が完了した後、 **[OK]** を選択します。

> [!NOTE]
> 各ログ バックアップは、復旧チェーンを形成するために、以前の完全バックアップにチェーンされています。 この完全バックアップは、前回のログ バックアップのリテンション期間が終了するまで保持されます。 これは、完全バックアップのリテンション期間を追加して、すべてのログが確実に復旧されるようにすることを意味します。 ユーザーが、週単位の完全バックアップ、日単位の差分、2 時間ごとのログを実行しているとしましょう。 これらのすべてが 30 日間保持されます。 ただし、週単位の完全バックアップは、次の完全バックアップが利用可能になった後でのみ、すなわち 30 + 7 日後に、実際にクリーンアップまたは削除することができます。 たとえば、週単位の完全バックアップが 11 月 16 日に行われたとします。 保持ポリシーに従って、12 月 16 日まで保持されます。 この完全バックアップに対する前回のログ バックアップは、11 月 22 日に予定されている次の完全バックアップの前に行われます。 このログが 12 月 22 日までに利用可能になるまでは、11 月 16 日の完全バックアップは削除されません。 そのため、11 月 16 日の完全バックアップは、12 月 22 日までは保持されます。

## <a name="run-an-on-demand-backup"></a>オンデマンド バックアップを実行する

バックアップは、ポリシー スケジュールに従って実行されます。 次のように、バックアップ オンデマンを実行できます。

1. コンテナー メニューで **[バックアップ項目]** を選択します。
2. **[バックアップ項目]** で、SAP HANA データベースを実行している VM を選択してから、 **[今すぐバックアップ]** を選択します。
3. **[今すぐバックアップ]** で、実行するバックアップの種類を選択します。 **[OK]** をクリックします。 このバックアップ項目に関連付けられているポリシーに従って、このバックアップが保持されます。
4. ポータルの通知を監視します。 コンテナー ダッシュボードの **[バックアップ ジョブ]**  >  **[進行中]** でジョブの進行状況を監視できます。 データベースのサイズによっては、最初のバックアップの作成に時間がかかる場合があります。

既定では、オンデマンド バックアップの保持期間は 45 日です。

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Azure Backup が有効になっているデータベースで SAP HANA Studio のバックアップを実行する

Azure Backup でバックアップされているデータベースの (HANA Studio を使用する) ローカル バックアップを作成する場合は、次の操作を行います。

1. データベースの完全バックアップまたはログ バックアップがすべて完了するまで待ちます。 SAP HANA Studio/Cockpit で状態を確認します。
1. ログ バックアップを無効にし、関連するデータベースのファイル システムにバックアップ カタログを設定します。
1. これを行うには、**systemdb** >  **[構成]**  >  **[データベースの選択]**  >  **[Filter (Log)]\(フィルター (ログ)\)** の順にダブルクリックします。
1. **[enable_auto_log_backup]** を **[No]** に設定します。
1. **[log_backup_using_backint]** を **[False]** に設定します。
1. **[catalog_backup_using_backint]** を **[False]** に設定します。
1. データベースのオンデマンド完全バックアップを作成します。
1. 完全バックアップとカタログ バックアップが完了するまで待ちます。
1. 前の設定を Azure のものに戻します。
    * **[enable_auto_log_backup]** を **[Yes]** に設定します。
    * **[log_backup_using_backint]** を **[True]** に設定します。
    * **[catalog_backup_using_backint]** を **[True]** に設定します。

## <a name="next-steps"></a>次のステップ

* [Azure VM で稼働している SAP HANA データベースを復元する](./sap-hana-db-restore.md)方法を学習する
* [Azure Backup を使用してバックアップされた SAP HANA データベースを管理する](./sap-hana-db-manage.md)方法を学習する
