---
title: Azure マネージド ディスクを復元する
description: Azure portal から Azure マネージド ディスクを復元する方法について説明します。
ms.topic: conceptual
ms.date: 01/07/2021
ms.openlocfilehash: 995217cd17d1e2a16cd7a5f963ee88aa7116d4a7
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703751"
---
# <a name="restore-azure-managed-disks-in-preview"></a>Azure マネージド ディスクを復元する (プレビュー段階)

>[!IMPORTANT]
>Azure ディスク バックアップは、サービス レベル アグリーメントのないプレビュー段階であるため、運用ワークロードにはお勧めできません。 詳しくは、[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)に関するページをご覧ください。 利用可能なリージョンについては、[サポート マトリックス](disk-backup-support-matrix.md)に関するページをご覧ください。
>
>プレビューにサインアップするには、[このフォームに記入](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR1vE8L51DIpDmziRt_893LVUNFlEWFJBN09PTDhEMjVHS05UWFkxUlUzUS4u)してください。

この記事では、Azure Backup によって作成された復元ポイントから [Azure マネージド ディスク](../virtual-machines/managed-disks-overview.md)を復元する方法について説明します。

現在、バックアップが作成された既存のソース ディスクを置き換えることによって復元する、元の場所への復旧 (OLR) オプションはサポートされていません。 復旧ポイントから復元して、バックアップが作成されたソース ディスクと同じリソース グループか、または他の任意のリソース グループに新しいディスクを作成できます。 これは、別の場所への復旧 (ALR) と呼ばれ、ソース ディスクと復元された (新しい) ディスクの両方を保持するのに役立ちます。

この記事では、次の方法について学習します。

- 復元して新しいディスクを作成する

- 復元操作の状態を追跡する

## <a name="restore-to-create-a-new-disk"></a>復元して新しいディスクを作成する

バックアップ コンテナーでは、マネージド ID を使用して他の Azure リソースにアクセスします。 バックアップから復元するには、バックアップ コンテナーのマネージド ID に、ディスクを復元する先のリソース グループに対する一連のアクセス許可が必要になります。

バックアップ コンテナーでは、リソースあたり 1 つに制限され、このリソースのライフサイクルに関連付けられているシステム割り当てマネージド ID を使用します。 マネージド ID には、Azure ロールベースのアクセス制御 (Azure RBAC) を使用してアクセス許可を付与できます。 マネージド ID は、Azure リソースでのみ使用できる特殊な種類のサービス プリンシパルです。 [マネージド ID](../active-directory/managed-identities-azure-resources/overview.md) の詳細を確認してください。

復元操作を実行するには、次の前提条件が必要です。

1. Azure Backup サービスによってディスクが復元されるリソース グループで、バックアップ コンテナーのマネージド ID に **[ディスク復元オペレーター]** ロールを割り当てます。

    >[!NOTE]
    > バックアップが作成されるソース ディスクと同じリソース グループか、あるいは同じサブスクリプションまたは別のサブスクリプション内の他の任意のリソース グループを選択できます。

    1. ディスクを復元する先のリソース グループに移動します。 たとえば、このリソース グループを *TargetRG* とします。

    1. **[アクセス制御 (IAM)]** に移動して、 **[ロールの割り当ての追加]** を選択します

    1. 右側のコンテキスト ペインで、 **[ロール]** ドロップダウン リストの **[ディスク復元オペレーター]** を選択します。 バックアップ コンテナーのマネージド ID を選択して **[保存]** を選択します。

        >[!TIP]
        >バックアップ コンテナーのマネージド ID を選択するには、そのコンテナーの名前を入力します。

        ![[ディスク復元オペレーター] ロールを選択する](./media/restore-managed-disks/disk-restore-operator-role.png)

1. ディスクが復元されるリソース グループで、バックアップ コンテナーのマネージド ID に適切な一連のロールの割り当てが設定されていることを確認します。

    1. **[バックアップ コンテナー] - > [ID]** の順に移動し、 **[Azure でのロールの割り当て]** を選択します。

        ![[Azure でのロールの割り当て] を選択する](./media/restore-managed-disks/azure-role-assignments.png)

    1. ロール、リソース名、リソースの種類が正しく表示されることを確認します。

        ![ロール、リソース名、リソースの種類を確認する](./media/restore-managed-disks/verify-role.png)

    >[!NOTE]
    >ポータルではロールの割り当てが正しく反映されますが、バックアップ コンテナーのマネージド ID にアクセス許可が適用されるまでに 15 分程度かかります。
    >
    >スケジュールされたバックアップまたはオンデマンド バックアップ操作中に、Azure Backup ではディスクの増分スナップショットを、そのディスクのバックアップの構成時に指定されたスナップショット リソース グループに格納します。 Azure Backup では、復元操作中にこれらの増分スナップショットを使用します。 スナップショットがスナップショット リソース グループから削除または移動されている場合、またはバックアップ コンテナーのロールの割り当てがスナップショット リソース グループで取り消されている場合、復元操作は失敗します。

1. 復元されるディスクが [カスタマー マネージド キー (CMK)](../virtual-machines/disks-enable-customer-managed-keys-portal.md) で暗号化されているか、[プラットフォーム マネージド キーとカスタマー マネージド キーによる二重暗号化](../virtual-machines/disks-enable-double-encryption-at-rest-portal.md)を使用して暗号化されている場合は、バックアップ コンテナーのマネージド ID に、**ディスク暗号化セット** リソースに対する **リーダー** ロールを割り当てます。

前提条件が満たされたら、次の手順に従って復元操作を実行します。

1. [Azure portal](https://portal.azure.com/) で、**バックアップ センター** に移動します。 **[管理]** セクションで **[バックアップ インスタンス]** を選択します。 バックアップ インスタンスの一覧から、復元操作を実行するディスク バックアップ インスタンスを選択します。

    ![バックアップ インスタンスの一覧](./media/restore-managed-disks/backup-instances.png)

    あるいは、ディスクのバックアップを構成するために使用したバックアップ コンテナーからこの操作を実行することもできます。

1. **[バックアップ インスタンス]** 画面で、復元操作を実行するために使用する復元ポイントを選択し、 **[復元]** を選択します。

    ![復元ポイントの選択](./media/restore-managed-disks/select-restore-point.png)

1. **[復元]** ワークフローで、 **[基本]** および **[復旧ポイントを選択する]** タブの情報を確認し、 **[次へ: 復元のパラメーター]** を選択します。

    ![[基本] と [復旧ポイントを選択する] の情報を確認する](./media/restore-managed-disks/review-information.png)

1. **[復元のパラメーター]** タブで、 **[ターゲット サブスクリプション]** と、バックアップを復元する先の **[ターゲット リソース グループ]** を選択します。 復元されるディスクの名前を指定します。 **Next:[次へ: レビューと復元]** を選択します。

    ![復元のパラメーター](./media/restore-managed-disks/restore-parameters.png)

    >[!TIP]
    >ディスク バックアップ ソリューションを使用して Azure Backup でバックアップされるディスクは、Azure VM バックアップ ソリューションを Recovery Services コンテナーと共に使用して Azure Backup でバックアップすることもできます。 このディスクが接続されている Azure VM の保護を構成している場合は、Azure VM の復元操作を使用することもできます。 対応する Azure VM バックアップ インスタンスの復旧ポイントから VM、またはディスクとファイルまたはフォルダーを復元することを選択できます。 詳細については、[Azure VM のバックアップ](./about-azure-vm-restore.md)に関するページを参照してください。

1. 検証が成功したたら、 **[復元]** を選択して復元操作を開始します。

    ![復元操作を開始する](./media/restore-managed-disks/initiate-restore.png)

    >[!NOTE]
    > 検証が完了して、復元操作をトリガーできるようになるまでに数分かかることがあります。 検証は、次の場合に失敗する可能性があります。
    >
    > - **[復元されたディスクの名前]** で指定されたものと同じ名前を持つディスクが **ターゲット リソース グループ** に既に存在する
    > - バックアップ コンテナーのマネージド ID の **ターゲット リソース グループ** に有効なロールの割り当てが含まれていない
    > - バックアップ コンテナーのマネージド ID のロールの割り当てが、増分スナップショットが格納される **[スナップショット リソース グループ]** で取り消されている
    > - 増分スナップショットがスナップショット リソース グループから削除または移動されている

復元では、選択された復旧ポイントからの新しいディスクが、復元操作中に指定されたターゲット リソース グループに作成されます。 復元されたディスクを既存の仮想マシンで使用するには、さらに次の手順を実行する必要があります。

- 復元されたディスクがデータ ディスクである場合は、既存のディスクを仮想マシンに接続できます。 復元されたディスクが OS ディスクである場合は、Azure portal の **[設定]** セクションの **[仮想マシン]** ペイン - > **[ディスク]** メニューから仮想マシンの OS ディスクをスワップできます。

    ![OS ディスクをスワップする](./media/restore-managed-disks/swap-os-disks.png)

- Windows 仮想マシンでは、復元されたディスクがデータ ディスクである場合、仮想マシンから[元のデータ ディスクを切断する](../virtual-machines/windows/detach-disk.md#detach-a-data-disk-using-the-portal)手順に従います。 その後、[復元されたディスクを仮想マシンに接続](../virtual-machines/windows/attach-managed-disk-portal.md)します。 復元されたディスクを使用して、仮想マシンの [OS ディスクをスワップする](../virtual-machines/windows/os-disk-swap.md)手順に従います。

- Linux 仮想マシンでは、復元されたディスクがデータ ディスクである場合、仮想マシンから[元のデータ ディスクを切断する](../virtual-machines/linux/detach-disk.md#detach-a-data-disk-using-the-portal)手順に従います。 その後、[復元されたディスクを仮想マシンに接続](../virtual-machines/linux/attach-disk-portal.md#attach-an-existing-disk)します。 復元されたディスクを使用して、仮想マシンの [OS ディスクをスワップする](../virtual-machines/linux/os-disk-swap.md)手順に従います。

復元操作が正常に完了したら、**ターゲット リソース グループ** で、バックアップ コンテナーのマネージド ID から **[ディスク復元オペレーター]** ロールの割り当てを取り消すことをお勧めします。

## <a name="track-a-restore-operation"></a>復元操作を追跡する

復元操作をトリガーすると、追跡用のジョブがバックアップ サービスによって作成されます。 Azure Backup は、ジョブに関する通知をポータルに表示します。 復元ジョブの進行状況を表示するには、次の手順を実行します。

1. **[バックアップ インスタンス]** 画面に移動します。 ここには、過去 7 日間の操作と状態を含むジョブのダッシュボードが表示されます。

    ![ジョブのダッシュボード](./media/restore-managed-disks/jobs-dashboard.png)

1. 復元操作の状態を表示するには、 **[すべて表示]** を選択して、このバックアップ インスタンスの進行中のジョブと過去のジョブを表示します。

    ![[すべて表示] を選択する](./media/restore-managed-disks/view-all.png)

1. バックアップおよび復元ジョブとそれらの状態の一覧を確認します。 ジョブの一覧からジョブを選択すると、そのジョブの詳細が表示されます。

    ![ジョブの一覧](./media/restore-managed-disks/list-of-jobs.png)

## <a name="next-steps"></a>次の手順

- [Azure ディスク バックアップに関する FAQ](disk-backup-faq.md)