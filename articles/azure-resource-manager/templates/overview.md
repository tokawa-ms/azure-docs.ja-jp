---
title: テンプレートの概要
description: リソースのデプロイに Azure Resource Manager テンプレート (ARM テンプレート) を使用する利点について説明します。
ms.topic: conceptual
ms.date: 03/08/2021
ms.openlocfilehash: 3a144b69f69c90b997b17d9526c9c3e0f259c554
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2021
ms.locfileid: "102455737"
---
# <a name="what-are-arm-templates"></a>ARM テンプレートとは

クラウドに移行するとき、多くのチームでアジャイル開発方法が採用されてきました。 このようなチームでは反復が迅速に行われます。 ソリューションをクラウドに繰り返しデプロイし、インフラストラクチャが信頼できる状態であることを把握する必要があります。 インフラストラクチャが反復的なプロセスの一部になると、運用と開発の間の区切りはなくなります。 チームは、統合されたプロセスを通してインフラストラクチャとアプリケーション コードを管理する必要があります。

これらの課題に対応するには、デプロイを自動化し、インフラストラクチャの手法をコードとして使用することができます。 コードでは、デプロイする必要があるインフラストラクチャを定義します。 インフラストラクチャのコードは、プロジェクトの一部になります。 アプリケーションのコードと同様に、インフラストラクチャのコードをソース リポジトリに格納し、そのバージョンを管理します。 チーム内の誰でもコードを実行して、同様の環境をデプロイできます。

Azure ソリューション用にインフラストラクチャをコードとして実装するには、Azure Resource Manager テンプレート (ARM テンプレート) を使用します。 テンプレートは JavaScript Object Notation (JSON) ファイルであり、プロジェクトのインフラストラクチャと構成が定義されています。 このテンプレートでは、デプロイしようとしているものを、それを作成する一連のプログラミング コマンドを記述しなくても記述できる、宣言型の構文を使用しています。 このテンプレートでは、デプロイするリソースとそれらのリソースのプロパティを指定します。

Microsoft では、ARM テンプレートを開発するための新しい言語を導入しました。 この言語は Bicep という名前で、現在はプレビュー段階にありです。 Bicep および JSON テンプレートは、同じ機能を提供します。 テンプレートは 2 つの言語間で変換できます。 Bicep には、テンプレートの作成に使用しやすい構文が用意されています。 詳細については、「[Bicep とは (プレビュー)](bicep-overview.md)」を参照してください。

## <a name="why-choose-arm-templates"></a>ARM テンプレートを選択する理由

ARM テンプレートを使用するか、他のいずれかのコードとしてのインフラストラクチャ サービスを使用するか決めるときに、テンプレートを使用した場合の以下の長所を考慮してください。

* **宣言型の構文**:ARM テンプレートを使用すると、Azure インフラストラクチャ全体を宣言方式で作成し、デプロイできます。 たとえば、仮想マシンだけでなく、ネットワーク インフラストラクチャ、ストレージ システム、場合によっては必要となるその他のリソースもデプロイできます。

* **反復可能結果**:インフラストラクチャを開発のライフサイクル全体で繰り返しデプロイし、常に一貫性のある方法でリソースをデプロイします。 テンプレートはべき等です。つまり、同じテンプレートを何度でもデプロイし、同じリソースの種類を同じ状態で作成できます。 更新を提示する個別のテンプレートをたくさん開発するのではなく、望ましい状態を提示するテンプレートを 1 つ開発できます。

* **オーケストレーション**:複雑な順序付けのことを心配する必要はありません。 Resource Manager では、相互に依存するリソースのデプロイが調整され、正しい順序で作成されます。 Resource Manager では、可能な箇所でリソースが並列デプロイされます。そのため、直列デプロイに比べ、短時間でデプロイが完了します。 テンプレートは複数の命令コマンドではなく、1 つのコマンドを使用してデプロイします。

   ![テンプレートのデプロイの比較](./media/overview/template-processing.png)

* **モジュール式のファイル**:テンプレートは再利用可能な構成部品に細分化し、デプロイ時にリンクさせることができます。 あるテンプレートを別のテンプレート内の入れ子にすることもできます。

* **Azure リソースの作成**:テンプレートでは、Azure の新しいサービスと機能をすぐに利用できます。 リソース プロバイダーによって新しいリソースが導入されたらすぐに、テンプレートを使用してこれらのリソースをデプロイできます。 新しいサービスは、ツールやモジュールが更新されるまで待たなくても使用することができます。

* **拡張性**: [デプロイ スクリプト](deployment-script-template.md)では、PowerShell または Bash スクリプトをテンプレートに追加できます。 デプロイ スクリプトを使用すると、デプロイ時にリソースを設定する機能が拡張されます。 スクリプトは、テンプレートに含めることも、外部ソースに格納してテンプレート内で参照することもできます。 デプロイ スクリプトを使用すると、1 つの ARM テンプレートでエンドツーエンド環境をセットアップできます。

* **テスト**: ARM テンプレート ツール キット (arm-ttk) を使用してテストすると、テンプレートが推奨ガイドラインに従っていることを確認できます。 このテスト キットは PowerShell スクリプトであり、[GitHub](https://github.com/Azure/arm-ttk) からダウンロードできます。 このツール キットを使用すると、テンプレート言語を使用して、より簡単に専門知識を開発できます。

* **変更のプレビュー**: テンプレートをデプロイする前に、[what-if 操作](template-deploy-what-if.md) を使用して、変更のプレビューを取得できます。 what-if では、どのリソースが作成、更新、または削除されるか、および変更されるリソース プロパティが表示されます。 what-if 操作により、環境の現在の状態が確認され、状態を管理する必要がなくなります。

* **組み込みの検証**:テンプレートは検証に合格しないとデプロイされません。 Resource Manager によって、デプロイ開始前にテンプレートが調べられ、デプロイが成功することが確認されます。 デプロイが半分完了した状態で停止する可能性が低くなります。

* **デプロイの追跡**:Azure portal でデプロイ履歴を確認し、テンプレートのデプロイに関する情報を取得できます。 デプロイされたテンプレート、渡されたパラメーター値、およびすべての出力値を確認できます。 他のコードとしてのインフラストラクチャ サービスは、ポータルでは追跡されません。

   ![デプロイ履歴](./media/overview/deployment-history.png)

* **コードとしてのポリシー**:[Azure Policy](../../governance/policy/overview.md) は、ガバナンスを自動化するためのコード フレームワークとしてのポリシーです。 Azure ポリシーを使用している場合、テンプレート経由でデプロイすると、非準拠リソースではポリシーが修復されます。

* **デプロイのブループリント**:Microsoft が提供する [ブループリント](../../governance/blueprints/overview.md)を活用することで、規制やコンプライアンスの基準を満たすことができます。 これらのブループリントには、さまざまなアーキテクチャ用の既成のテンプレートが含まれています。

* **CI/CD の統合**: テンプレートを継続的インテグレーションと継続的配置 (CI/CD) ツールに統合することができます。これにより、リリース パイプラインを自動化し、高速で信頼性の高いアプリケーションとインフラストラクチャの更新を行うことができます。 Azure DevOps と Resource Manager テンプレート タスクを使うことで、Azure Pipelines を使用して ARM テンプレート プロジェクトを継続的にビルドおよびデプロイすることができます。 詳細については、[パイプラインを使用した VS プロジェクト](add-template-to-azure-pipelines.md)に関するページおよび「[チュートリアル: Azure Pipelines を使用した Azure Resource Manager テンプレートの継続的インテグレーション](./deployment-tutorial-pipeline.md)」を参照してください。

* **エクスポート可能なコード**:リソース グループの現在の状態をエクスポートするか、特定のデプロイに使用されたテンプレートを表示することで、既存のリソース グループのテンプレートを取得できます。 [エクスポートしたテンプレート](export-template-portal.md)を表示すると、テンプレートの構文について理解するのに役立ちます。

* **作成ツール**:[Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md) とテンプレート ツール拡張機能でテンプレートを作成できます。 Intellisense、構文の強調表示、インライン ヘルプ、その他の多くの言語機能が与えられます。 Visual Studio Code に加えて、[Visual Studio](create-visual-studio-deployment-project.md) を使用することもできます。

## <a name="template-file"></a>テンプレート ファイル

テンプレート内では、JSON の機能を拡張する[テンプレート式](template-expressions.md)を記述できます。 これらの式では、Resource Manager によって提供される[関数](template-functions.md)を使用します。

テンプレートには次のセクションがあります。

* [パラメーター](template-parameters.md) - 同じテンプレートを異なる環境で使用できるように、デプロイ時に値を指定します。

* [変数](template-variables.md) - テンプレートで再利用される値を定義します。 パラメーター値から構築できます。

* [ユーザー定義関数](template-user-defined-functions.md) - テンプレートを簡略化するカスタマイズされた関数を作成します。

* [リソース](resource-declaration.md) - デプロイするリソースを指定します。

* [出力](template-outputs.md) - デプロイされたリソースから値を返します。

## <a name="template-deployment-process"></a>テンプレートのデプロイ手順

テンプレートをデプロイするときは、Resource Manager によってテンプレートが REST API 操作に変換されます。 たとえば、Resource Manager が次のリソース定義を含むテンプレートを受け取ったとします。

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2019-04-01",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "StorageV2",
    "properties": {}
  }
]
```

Resource Manager は、この定義を次の REST API 操作に変換し、Microsoft.Storage リソース プロバイダーに送信します。

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2019-04-01
REQUEST BODY
{
  "location": "westus",
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "StorageV2",
  "properties": {}
}
```

リソースに対してテンプレートで設定された **apiVersion** が、REST 操作の API バージョンとして使用されていることに注意してください。 テンプレートを繰り返しデプロイし、自信を持って引き続き動作させることができます。 同じ API バージョンを使用することで、新しいバージョンで導入される可能性がある破壊的変更について気にする必要はなくなります。

テンプレートをデプロイするには、次のいずれかのオプションを使用します。

* [Azure Portal](deploy-portal.md)
* [Azure CLI](deploy-cli.md)
* [PowerShell](deploy-powershell.md)
* [REST API](deploy-rest.md)
* [GitHub リポジトリのボタン](deploy-to-azure-button.md)
* [Azure Cloud Shell](deploy-cloud-shell.md)

## <a name="template-design"></a>テンプレートの設計

テンプレートとリソース グループをどのように定義するかは、ソリューションの管理方法に応じてユーザーが自由に決定できます。 たとえば、1 つのリソース グループに 1 つのテンプレートで 3 層のアプリケーションをデプロイできます。

![3 層のテンプレート](./media/overview/3-tier-template.png)

ただし、インフラストラクチャ全体を 1 つのテンプレートで定義する必要はありません。 多くの場合、対象を絞って目的を特化した一連のテンプレートにデプロイの要件を分類することが合理的です。 これらのテンプレートは、さまざまなソリューションで簡単に再利用できます。 特定のソリューションをデプロイするには、必要なすべてのテンプレートをリンクするメイン テンプレートを作成します。 次の画像は、入れ子になった 3 つのテンプレートを含む親テンプレートを利用して 3 層のソリューションをデプロイする方法を示しています。

![入れ子になったテンプレートの層](./media/overview/nested-tiers-template.png)

層ごとに異なるライフサイクルを希望する場合は、3 層を個別のリソース グループにデプロイできます。 リソースは、他のリソース グループ内のリソースにリンクされることもあります。

![テンプレートの層](./media/overview/tier-templates.png)

入れ子になったテンプレートについては、「[Azure Resource Manager でのリンクされたテンプレートの使用](linked-templates.md)」を参照してください。

## <a name="share-templates"></a>テンプレートの共有

テンプレートは、作成した後で組織内の他のユーザーと共有することができます。 [テンプレート スペック](template-specs.md)では、テンプレートをリソースの種類として保管することができます。 テンプレート スペックへのアクセス権を管理するには、ロールベースのアクセス制御を使用します。テンプレート スペックへの読み取りアクセス権を持つユーザーはテンプレートをデプロイできますが、変更することはできません。

つまり、このアプローチでは組織の標準を満たすテンプレートを安全に共有できます。

## <a name="next-steps"></a>次のステップ

* テンプレートの作成手順について説明したチュートリアルについては、「[チュートリアル:初めての ARM テンプレートを作成してデプロイする](template-tutorial-create-first-template.md)」を参照してください。
* Microsoft Learn のガイド付きモジュール セットを使用して ARM テンプレートを学習するには、「[ARM テンプレートを使用して Azure にリソースをデプロイして管理する」](/learn/paths/deploy-manage-resource-manager-templates/)を参照してください。
* テンプレート ファイルのプロパティについては、「[ARM テンプレートの構造と構文の詳細](template-syntax.md)」を参照してください。
* テンプレートのエクスポートの詳細については、「[クイックスタート:Azure portal を使用して ARM テンプレートを作成およびデプロイする](quickstart-create-templates-use-the-portal.md)」を参照してください。
* 一般的な質問に対する回答については、「[ARM テンプレートに関してよく寄せられる質問](frequently-asked-questions.md)」を参照してください。
