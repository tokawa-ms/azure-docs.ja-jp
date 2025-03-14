---
title: Azure セキュリティ ベンチマーク基盤ブループリント サンプルのデプロイ
description: ブループリント アーティファクト パラメーターの詳細を含む Azure セキュリティ ベンチマーク基盤ブループリント サンプルのデプロイ手順です。
ms.date: 03/12/2021
ms.topic: sample
ms.openlocfilehash: af41dd50c976ac6c0570b8a089211fa310ef4ef1
ms.sourcegitcommit: ec39209c5cbef28ade0badfffe59665631611199
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103232615"
---
# <a name="deploy-the-azure-security-benchmark-foundation-blueprint-sample"></a>Azure セキュリティ ベンチマーク基盤ブループリント サンプルのデプロイ

Azure セキュリティ ベンチマーク基盤ブループリント サンプルをデプロイするには、次の手順を実行する必要があります。

> [!div class="checklist"]
> - サンプルから新しいブループリントを作成する
> - サンプルのコピーを **発行済み** としてマークする
> - ブループリントのコピーを既存のサブスクリプションに割り当てる

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free) を作成してください。

## <a name="create-blueprint-from-sample"></a>サンプルからブループリントを作成する

最初に、サンプルをスターターとして使用して環境内に新しいブループリントを作成することにより、ブループリント サンプルを実装します。

1. 左側のウィンドウにある **[すべてのサービス]** を選択します。 **[ブループリント]** を探して選択します。

1. 左側の **[はじめに]** ページで、 _[ブループリントの作成]_ の下にある **[作成]** ボタンを選択します。

1. _[その他のサンプル]_ から **Azure セキュリティ ベンチマーク基盤** ブループリント サンプルを見つけて、 **[このサンプルを使用する]** を選択します。

1. ブループリント サンプルの _[基本]_ を入力します。

   - **[ブループリントの名前]** : Azure セキュリティ ベンチマーク基盤ブループリント サンプルのコピーの名前を指定します。
   - **[定義の場所]** :省略記号を使用して、サンプルのコピーを保存する管理グループを選択します。

1. ページの上部にある _[アーティファクト]_ タブまたはページの下部にある **[次へ: アーティファクト]** を選択します。

1. ブループリント サンプルを構成するアーティファクトの一覧を確認します。 多くのアーティファクトには、後で定義するパラメーターがあります。 ブループリント サンプルの確認が完了したら、 **[下書きの保存]** を選択します。

## <a name="publish-the-sample-copy"></a>サンプルのコピーを発行する

環境でのブループリント サンプルのコピーの作成が完了しました。 これは **下書き** モードで作成されており、割り当ておよびデプロイの前に **発行** する必要があります。 ブループリント サンプルのコピーは、お使いの環境や必要性に応じてカスタマイズできますが、それが原因で Azure セキュリティ ベンチマーク基盤ブループリントの条件が満たされなくなる場合があります。

1. 左側のウィンドウにある **[すべてのサービス]** を選択します。 **[ブループリント]** を探して選択します。

1. 左側の **[ブループリントの定義]** ページを選択します。 ブループリント サンプルのコピーを、フィルターを使用して検索し、選択します。

1. ページの上部にある **[ブループリントを発行する]** を選択します。 右側の新しいページで、ブループリント サンプルのコピーの **[バージョン]** を指定します。 このプロパティは、後で変更を加える場合に役立ちます。 **[変更に関するメモ]** に入力します (例: 「Azure セキュリティ ベンチマーク基盤ブループリント サンプルから発行する最初のバージョン」)。 次に、ページの下部にある **[発行]** を選択します。

## <a name="assign-the-sample-copy"></a>サンプルのコピーを割り当てる

正常に **発行** されたブループリント サンプルのコピーは、保存先の管理グループ内のサブスクリプションに割り当てることができます。 この手順では、ブループリント サンプルのコピーの各デプロイを一意にするためのパラメーターを指定します。

1. 左側のウィンドウにある **[すべてのサービス]** を選択します。 **[ブループリント]** を探して選択します。

1. 左側の **[ブループリントの定義]** ページを選択します。 ブループリント サンプルのコピーを、フィルターを使用して検索し、選択します。

1. ブループリント定義ページの上部にある **[ブループリントの割り当て]** を選択します。

1. ブループリント割り当て用のパラメーター値を指定します。

   - 基本
       - **サブスクリプション**:ブループリント サンプルのコピーを保存した管理グループ内の 1 つ以上のサブスクリプションを選択します。 複数のサブスクリプションを選択すると、入力したパラメーターを使用して、それぞれに対して割り当てが作成されます。
     - **割り当て名**:名前は、ブループリントの名前に基づいてあらかじめ設定されています。
       必要に応じて変更することも、そのままにしておくこともできます。
     - **[場所]** :マネージド ID を作成するリージョンを選択します。
     - Azure Blueprint は、この管理対象 ID を使用して、割り当てられたブループリント内にすべての成果物をデプロイします。
       詳細については、[Azure リソースの管理対象 ID の概要](../../../../active-directory/managed-identities-azure-resources/overview.md)に関するページをご覧ください。
     - **ブループリント定義ラベル**:ブループリント サンプルのコピーの **発行済み** バージョンを選択します。

   - ロックの割り当て

     お使いの環境のブループリントのロック設定を選択します。 詳細については、[ブループリント リソースのロック](../../concepts/resource-locking.md)に関するページを参照してください。

   - マネージド ID

     既定の _システム割り当て_ マネージド ID オプションまたは _ユーザー割り当て_ ID オプションを選択します。

   - ブループリントのパラメーター

     このセクションで定義するパラメーターは、一貫性を維持するために、ブループリント定義のアーティファクトの多くで使用されます。
    
     - **Prefix for resources and resource groups (リソースとリソース グループのプレフィックス)** : この文字列は、すべてのリソースおよびリソース グループの名前のプレフィックスとして使用されます
     - **ハブ名**: ハブの名前
     - **ログの保有期間 (日数)** : ログを保有する日数。"0" を入力すると、ログが無期限に保有されます
     - **ハブのデプロイ**: "true" または "false" を入力して、アーキテクチャのハブ コンポーネントが割り当てによってデプロイされるかどうかを指定します
     - **ハブの場所**: ハブ リソース グループの場所
     - **宛先 IP アドレス**: 送信接続用の宛先 IP アドレス。IP アドレスまたは IP 範囲プレフィックスのコンマ区切りの一覧
     - **Network Watcher 名**: Network Watcher リソースの名前
     - **Network Watcher リソース グループ名**: Network Watcher リソース グループの名前
     - **DDoS Protection の有効化**: "true" または "false" を入力して、仮想ネットワークで DDoS Protection を有効にするかどうかを指定します
     
    > [!NOTE] 
    > Network Watcher が既に有効になっている場合は、既存の Network Watcher リソース グループを使用することをお勧めします。 また、アーティファクト パラメーター **[Network Watcher リソース グループの場所]** に、既存の Network Watcher リソース グループの場所を指定する必要もあります。

   - アーティファクトのパラメーター

     このセクションで定義するパラメーターは、定義対象のアーティファクトに適用されます。 これらのパラメーターはブループリントの割り当て時に定義されるので、[動的パラメーター](../../concepts/parameters.md#dynamic-parameters)です。 アーティファクトのパラメーターとその説明を含む詳しい一覧については、「[アーティファクトのパラメーター表](#artifact-parameters-table)」を参照してください。

1. すべてのパラメーターの入力が完了したら、ページの下部にある **[割り当て]** を選択します。 ブループリントの割り当てが作成され、アーティファクトのデプロイが開始されます。 デプロイに要する時間は、約 1 時間です。 デプロイの状態を確認するには、ブループリントの割り当てを開きます。

> [!WARNING]
> Azure Blueprints サービスと、組み込まれているブループリント サンプルは、**無料** でご利用になれます。 Azure リソースは、[製品ごとに課金](https://azure.microsoft.com/pricing/)されます。 このブループリント サンプルでデプロイされるリソースの実行コストを見積もるには、[料金計算ツール](https://azure.microsoft.com/pricing/calculator/)を使用します。

## <a name="artifact-parameters-table"></a>アーティファクトのパラメーター表

次の表に、ブループリントのパラメーターを示します。

|アーティファクト名|アーティファクトの種類|パラメーター名|説明|
|-|-|-|-|
|ハブ リソース グループ|Resource group|リソース グループ名|ロック済み - プレフィックスとハブ名を連結します|
|ハブ リソース グループ|Resource group|リソース グループの場所|ロック済み - ハブの場所を使用します|
|Azure Firewall テンプレート|Resource Manager テンプレート|Azure Firewall のプライベート IP アドレス||
|Azure Log Analytics および Diagnostics テンプレート|Resource Manager テンプレート|Log Analytics ワークスペースの場所|Log Analytics ワークスペースが作成される場所。Azure PowersShell で `Get-AzLocation | Where-Object Providers -like 'Microsoft.OperationalInsights' | Select DisplayName` を実行して、使用可能なリージョンを確認します|
|Azure Log Analytics および Diagnostics テンプレート|Resource Manager テンプレート|Azure Automation アカウント ID (省略可能)|Automation アカウント リソース ID。Log Analytics と Automation アカウントの間でリンクされたサービスを作成するために使用します|
|Azure ネットワーク セキュリティ グループ テンプレート|Resource Manager テンプレート|NSG フロー ログの有効化|NSG フロー ログを有効または無効にするには、"true" または "false" を入力します|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|仮想ネットワーク アドレス プレフィックス|ハブ仮想ネットワークの仮想ネットワーク アドレス プレフィックス|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|ファイアウォール サブネット アドレス プレフィックス|ハブ仮想ネットワークのファイアウォール サブネット アドレス プレフィックス|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|要塞サブネット アドレス プレフィックス|ハブ仮想ネットワークの要塞サブネット アドレス プレフィックス|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|ゲートウェイ サブネット アドレス プレフィックス|ハブ仮想ネットワークのゲートウェイ サブネット アドレス プレフィックス|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|管理サブネット アドレス プレフィックス|ハブ仮想ネットワークの管理サブネット アドレス プレフィックス|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|ジャンプ ボックス サブネット アドレス プレフィックス|ハブ仮想ネットワークのジャンプ ボックス サブネット アドレス プレフィックス|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|サブネットのアドレス名 (省略可能)|ハブ仮想ネットワークにデプロイするサブネット名の配列 (例: "subnet1"、"subnet2")|
|Azure Virtual Network ハブ テンプレート|Resource Manager テンプレート|サブネット アドレス プレフィックス (省略可能)|ハブ仮想ネットワークのオプション サブネットの IP アドレス プレフィックスの配列 (例: "10.0.7.0/24"、"10.0.8.0/24")|
|スポーク リソース グループ|Resource group|リソース グループ名|ロック済み - プレフィックスとスポーク名を連結します|
|スポーク リソース グループ|Resource group|リソース グループの場所|ロック済み - ハブの場所を使用します|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|スポークのデプロイ|"true" または "false" を入力して、アーキテクチャのスポーク コンポーネントが割り当てによってデプロイされるかどうかを指定します|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|ハブ サブスクリプション ID|ハブがデプロイされているサブスクリプション ID。既定値は、ブループリント定義が配置されているサブスクリプションです|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|スポーク名|スポークの名前|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|仮想ネットワーク アドレス プレフィックス|スポーク仮想ネットワークの仮想ネットワーク アドレス プレフィックス|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|サブネット アドレス プレフィックス|スポーク仮想ネットワークのサブネット アドレス プレフィックス|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|サブネットのアドレス名 (省略可能)|スポーク仮想ネットワークにデプロイするサブネット名の配列 (例: "subnet1"、"subnet2")|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|サブネット アドレス プレフィックス (省略可能)|スポーク仮想ネットワークのオプション サブネットの IP アドレス プレフィックスの配列 (例: "10.0.7.0/24"、"10.0.8.0/24")|
|Azure Virtual Network スポーク テンプレート|Resource Manager テンプレート|スポークのデプロイ|"true" または "false" を入力して、アーキテクチャのスポーク コンポーネントが割り当てによってデプロイされるかどうかを指定します|
|Azure Network Watcher テンプレート|Resource Manager テンプレート|Network Watcher の場所|Network Watcher リソースの場所|
|Azure Network Watcher テンプレート|Resource Manager テンプレート|Network Watcher リソース グループの場所|Network Watcher が既に有効になっている場合、このパラメーター値は既存の Network Watcher リソース グループの場所と一致している **必要があります**。|

## <a name="troubleshooting"></a>トラブルシューティング

`The resource group 'NetworkWatcherRG' failed to deploy due to the
following error: Invalid resource group location '{location}'. The Resource group already exists in
location '{location}'.` というエラーが発生した場合は、ブループリント パラメーター **[Network Watcher リソース グループ名]** に既存の Network Watcher リソース グループ名が指定されていること、およびアーティファクト パラメーター **[Network Watcher リソース グループの場所]** に既存の Network Watcher リソース グループの場所が指定されていることを確認します。

## <a name="next-steps"></a>次の手順

Azure セキュリティ ベンチマーク基盤ブループリント サンプルをデプロイする手順を確認したので、以下の記事に進み、アーキテクチャの詳細を確認します。

> [!div class="nextstepaction"]
> [Azure セキュリティ ベンチマーク基盤ブループリント - 概要](./index.md)

ブループリントとその使用方法に関するその他の記事:

- [ブループリントのライフサイクル](../../concepts/lifecycle.md)を参照する。
- [静的および動的パラメーター](../../concepts/parameters.md)の使用方法を理解する。
- [ブループリントの優先順位](../../concepts/sequencing-order.md)のカスタマイズを参照する。
- [ブループリントのリソース ロック](../../concepts/resource-locking.md)の使用方法を調べる。
- [既存の割り当ての更新](../../how-to/update-existing-assignments.md)方法を参照する。
