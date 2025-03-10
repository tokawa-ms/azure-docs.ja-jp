---
title: インクルード ファイル
description: インクルード ファイル
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/04/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9ec5920295117b1726a684bcd08534cd6917f7e5
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "100569791"
---
1. Azure portal メニューまたは **[ホーム]** ページで、 **[リソースの作成]** を選択します。

1. **[新規]** ページで、 **[計算]** 、 >  **[関数アプリ]** の順に選択します。

1. **[基本]** ページで、下の表で指定されている関数アプリの設定を使用します。

    | 設定      | 推奨値  | 説明 |
    | ------------ | ---------------- | ----------- |
    | **サブスクリプション** | 該当するサブスクリプション | この新しい Function App が作成されるサブスクリプション。 |
    | **[リソース グループ](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Function App を作成するための新しいリソース グループの名前。 |
    | **関数アプリ名** | グローバルに一意の名前 | 新しい Function App を識別する名前。 有効な文字は、`a-z` (大文字と小文字の区別をしない)、`0-9`、および `-`です。  |
    |**発行**| コード | コード ファイルまたは Docker コンテナーの発行オプション。 |
    | **ランタイム スタック** | 優先言語 | お気に入りの関数プログラミング言語をサポートするランタイムを選択します。 ポータル内編集は、JavaScript、PowerShell、TypeScript、および C# スクリプトでのみ利用できます。 C# クラス ライブラリ、Java、Python の関数は[ローカルで開発](../articles/azure-functions/functions-develop-local.md#local-development-environments)する必要があります。  |
    |**Version**| バージョン番号 | インストールされているランタイムのバージョンを選択します。 |
    |**リージョン**| 優先リージョン | ユーザーに近い[リージョン](https://azure.microsoft.com/regions/)、または関数がアクセスする他のサービスの近くのリージョンを選択します。 |

1. **[次へ :ホスティング]** を選択します。 **[ホスティング]** ページで、次の設定を入力します。

    | 設定      | 推奨値  | 説明 |
    | ------------ | ---------------- | ----------- |
    | **[ストレージ アカウント](../articles/storage/common/storage-account-create.md)** |  グローバルに一意の名前 |  Function App で使用されるストレージ アカウントを作成します。 ストレージ アカウント名は、3 文字から 24 文字までの長さにし、数字と小文字のみを使用する必要があります。 既存のアカウントを使用することもできますが、[ストレージ アカウントの要件](../articles/azure-functions/storage-considerations.md#storage-account-requirements)を満たしている必要があります。 |
    |**オペレーティング システム**| Windows | オペレーティング システムは、ランタイム スタックの選択に基づいてあらかじめ選択されますが、必要に応じて設定を変更できます。 ポータルでの編集は Windows でのみサポートされます。 |
    | **[プラン](../articles/azure-functions/functions-scale.md)** | **従量課金プラン (サーバーレス)** | Function App にどのようにリソースが割り当てられるかを定義するホスティング プラン。 既定の **[従量課金プラン]** では、リソースは関数の必要に応じて動的に追加されます。 この[サーバーレス](https://azure.microsoft.com/overview/serverless-computing/)のホスティングでは、関数が実行された時間にのみ課金されます。 App Service プランで実行する場合は、[関数アプリのスケーリング](../articles/azure-functions/functions-scale.md)を管理する必要があります。  |

1. **[次へ :監視]** を選択します。 **[監視]** ページで、次の設定を入力します。

    | 設定      | 推奨値  | 説明 |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Default | 最も近いサポートされているリージョン内に同じ *アプリ名* の Application Insights リソースを作成します。 この設定を展開するか、 **[新規作成]** を選択することによって、Application Insights 名を変更するか、データを格納する [Azure 地理的環境](https://azure.microsoft.com/global-infrastructure/geographies/)内の別のリージョンを選択することができます。 |

1. **[確認および作成]** を選択して、アプリ構成の選択内容を確認します。

1. **[確認および作成]** ページで設定を確認して、 **[作成]** を選択し、関数アプリをプロビジョニングしてデプロイします。

1. ポータルの右上隅の **[通知]** アイコンを選択し、"**デプロイメントに成功しました**" というメッセージが表示されるまで待ちます。

1. **[リソースに移動]** を選択して、新しい Function App を確認します。 また、 **[ダッシュボードにピン留めする]** を選択することもできます。 ピン留めすると、ダッシュボードからこの関数アプリ リソースに戻るのが容易になります。

    ![デプロイの通知](./media/functions-create-function-app-portal/function-app-create-notification2.png)
