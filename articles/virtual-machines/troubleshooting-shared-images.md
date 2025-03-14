---
title: Azure での共有イメージに関する問題のトラブルシューティング
description: 共有イメージ ギャラリーに関する問題のトラブルシューティングを行う方法について説明します。
author: olayemio
ms.service: virtual-machines
ms.subservice: shared-image-gallery
ms.topic: troubleshooting
ms.workload: infrastructure
ms.date: 10/27/2020
ms.author: olayemio
ms.reviewer: cynthn
ms.openlocfilehash: d80caf767d923ce2539ca254a8312371155a3104
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2021
ms.locfileid: "102553733"
---
# <a name="troubleshoot-shared-image-galleries-in-azure"></a>Azure の共有イメージ ギャラリーのトラブルシューティング

共有イメージ ギャラリー、イメージ定義、およびイメージ バージョンで何らかの操作を実行しているときに問題が発生した場合は、失敗したコマンドをデバッグ モードで再実行します。 Azure CLI では `--debug` スイッチ、PowerShell では `-Debug` スイッチを渡して、デバッグ モードをアクティブにします。 エラーを特定したら、この記事に従ってトラブルシューティングします。


## <a name="creating-or-modifying-a-gallery"></a>ギャラリーの作成または変更 ##

**メッセージ**: "*ギャラリー名が無効です。許可されている文字はアンダースコアを含む、合計 80 文字までの英数字で、間にピリオドを使用できます。ダッシュを含むその他の特殊文字はすべて許可されません。* "  
**原因**:ギャラリーの名前が、名前付けの要件を満たしていません。  
**回避策**:次の条件に一致する名前を選択します: 
- 80 文字に制限されている
- 英字、数字、アンダースコア、ピリオドのみを含む
- 先頭と末尾が英字または数字である

**メッセージ**: "*その検証規則によると、エンティティ名 'galleryName' は無効です: ^[^\_\W][\w-.\_]{0,79}(?<![-.])$。* "  
**原因**:ギャラリー名が名前付けの要件を満たしていません。  
**回避策**:次の条件を満たすギャラリーの名前を選択します: 
- 80 文字に制限されている
- 英字、数字、アンダースコア、ピリオドのみを含む
- 先頭と末尾が英字または数字である

**メッセージ**: "*指定されたリソース名 <galleryName\> の末尾に、次の無効な文字が含まれています: <character\>。名前の末尾を次の文字にすることはできません: <character\>* "  
**原因**:ギャラリー名の末尾がピリオドまたはアンダースコアになっています。  
**回避策**:次の条件を満たすギャラリーの名前を選択します: 
- 80 文字に制限されている
- 英字、数字、アンダースコア、ピリオドのみを含む
- 先頭と末尾が英字または数字である

**メッセージ**: "*指定された場所 <region\> は、リソースの種類 'Microsoft.Compute/galleries' では使用できません。リソースの種類に使用できるリージョンの一覧は ...* "  
**原因**:ギャラリーに対して指定されたリージョンが正しくないか、アクセス要求が必要です。  
**回避策**: リージョン名が正しいことを確認します。 リージョン名が正しい場合は、そのリージョンに対して[アクセス要求](/troubleshoot/azure/general/region-access-request-process)を送信します。

**メッセージ**: "*入れ子になっているリソースを削除する前にリソースを削除することはできません。* "  
**原因**:少なくとも 1 つの既存のイメージ定義を含むギャラリーを削除しようとしました。 ギャラリーは削除する前に空にしておく必要があります。  
**回避策**:ギャラリー内のすべてのイメージ定義を削除してから、ギャラリーの削除に進みます。 イメージ定義にイメージ バージョンが含まれている場合は、イメージ定義を削除する前にイメージ バージョンを削除する必要があります。

**メッセージ**: "*ギャラリー名 '<galleryName\>' がサブスクリプション '<subscriptionId>' 内で一意ではありません。他のギャラリー名を選択してください。* "  
**原因**: 同じ名前の既存のギャラリーがあり、同じ名前で別のギャラリーを作成しようとしました。  
**回避策**: ギャラリーに別の名前を選択します。

**メッセージ**: "*リソース <galleryName\> は、場所 <region\_1\> のリソース グループ <resourceGroup\> に既に存在します。場所 <region\_2\> に同じ名前のリソースを作成することはできません。新しいリソース名を選択してください。* "  
**原因**: 同じ名前の既存のギャラリーがあり、同じ名前で別のギャラリーを作成しようとしました。  
**回避策**: ギャラリーに別の名前を選択します。

## <a name="creating-or-modifying-image-definitions"></a>イメージ定義の作成または変更 ##

**メッセージ**: "*プロパティ 'galleryImage.properties.<property\>' の変更は許可されていません。* "  
**原因**:OS の種類、OS の状態、Hyper-V の世代、プラン、公開元、SKU を変更しようとしました。 これらのプロパティはいずれも変更することはできません。  
**回避策**:代わりに、新しいイメージ定義を作成します。

**メッセージ**: "*リソース <galleryName/imageDefinitionName\> は、場所 <region\_1\> のリソース グループ<resourceGroup\> に既に存在します。場所 <region\_2\> に同じ名前のリソースを作成することはできません。新しいリソース名を選択してください。* "  
**原因**:同じギャラリーおよびリソース グループに同じ名前で既存のイメージ定義があります。 別のリージョンで同じギャラリーに同じ名前で別のイメージ定義を作成しようとしました。  
**回避策**:イメージ定義に別の名前を使用するか、イメージ定義を別のギャラリーまたはリソース グループに配置します。

**メッセージ**: "*指定されたリソース名 <galleryName\>/<imageDefinitionName\> の末尾に、次の無効な文字が含まれています: <character\>。名前の末尾を次の文字にすることはできません: <character\>* "  
**原因**:<imageDefinitionName\> の名前の末尾がピリオドまたはアンダースコアになっています。  
**回避策**:次の条件を満たすイメージ定義の名前を選択します: 
- 80 文字に制限されている
- 英字、数字、アンダースコア、ハイフン、ピリオドのみを含む
- 先頭と末尾が英字または数字である。

**メッセージ**: "*その検証規則によると、エンティティ名 <imageDefinitionName\> は無効です: ^[^\_\\W][\\w-.\_]{0,79}(?<![-.])$"* "  
**原因**:<imageDefinitionName\> の名前の末尾がピリオドまたはアンダースコアになっています。  
**回避策**:次の条件を満たすイメージ定義の名前を選択します: 
- 80 文字に制限されている
- 英字、数字、アンダースコア、ハイフン、ピリオドのみを含む
- 先頭と末尾が英字または数字である

**メッセージ**: "*資産名 galleryImage.properties.identifier.<property\> が有効ではありません。空にすることはできません。許可されている文字は、大文字または小文字、数字、ハイフン (-)、ピリオド (.)、アンダースコア (\_) です。名前の末尾にピリオド (.) を使用することはできません。名前の長さは、<number\> 文字を超えることはできません。* "  
**原因**:公開元、プラン、または SKU の値が、名前付けの要件を満たしていません。  
**回避策**:次の条件を満たす値を選択します: 
- 公開元は 128 文字、プランと SKU は 64 文字に制限されている
- 英字、数字、ハイフン、アンダースコア、ピリオドのみを含む
- 末尾がピリオドではない

**メッセージ**: "*ネストされたリソースに対して要求された操作を実行できません。親リソース <galleryName\> が見つかりません。* "  
**原因**:現在のサブスクリプションとリソース グループに <galleryName\> という名前のギャラリーがありません。  
**回避策**:ギャラリー、サブスクリプション、およびリソース グループの名前が正しいことを確認します。 それ以外の場合は、<galleryName\> という名前の新しいギャラリーを作成します。

**メッセージ**: "*指定された場所 <region\> は、リソースの種類 'Microsoft.Compute/galleries' では使用できません。リソースの種類に使用できるリージョンの一覧は ...* "  
**原因**:<region\> の名前が正しくないか、アクセス要求が必要です。  
**回避策**:リージョン名のスペルが正しいことを確認します。 このコマンドを実行して、アクセス権のあるリージョンを確認できます。 そのリージョンが一覧にない場合は、[アクセス要求](/troubleshoot/azure/general/region-access-request-process)を送信します。

**メッセージ**: "*値 <value\> を型 'iso-8601' としてシリアル化できません。ISO8601Error: ISO 8601 時間指定子 'T' が欠落しています。datetime 文字列 <value\> を解析できません*"  
**原因**:プロパティに指定された値が、日付として適切に書式設定されていません。  
**回避策**:yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz または [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) の有効な形式で日付を指定します。

**メッセージ**: "*文字列を DateTimeOffset <value\> に変換できませんでした。パス 'properties.<property\>'* "  
**原因**:プロパティに指定された値が、日付として適切に書式設定されていません。  
**回避策**:yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz または [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) の有効な形式で日付を指定します。

**メッセージ**: "*EndOfLifeDate は、未来の日付に設定する必要があります。* "  
**原因**:有効期間の終了日のプロパティが、今日の日付より後の日付として適切に書式設定されていません。  
**回避策**:yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz または [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) の有効な形式で日付を指定します。

**メッセージ**: "*引数 --<property\>: 無効な int 値: <value\>* "  
**原因**: <property\> には整数値のみを指定でき、<value\> は整数ではありません。  
**回避策**:整数値を選択します。

**メッセージ**: " *<property\> の最小値は <property\> の最大値より大きくすることはできません。* "  
**原因**:<property\> に指定された最小値が、<property\> に指定された最大値を超えています。  
**回避策**:最小値が最大値以下になるように値を変更します。

**メッセージ**: "*ギャラリー イメージ: (公開元: <Publisher\>、プラン: <Offer\>、SKU: <SKU\>) で識別された <imageDefinitionName\> は既に存在します。別の公開元、プラン、sku の組み合わせを選択してください。* "  
**原因**:同じギャラリーに、公開元、プラン、SKU の 3 つが既存のイメージ定義と同じである新しいイメージ定義を作成しようとしました。  
**回避策**:1 つのギャラリー内では、すべてのイメージ定義で公開元、プラン、SKU の組み合わせは一意である必要があります。 一意の組み合わせを選択するか、新しいギャラリーを選択して、イメージ定義を再度作成します。

**メッセージ**: "*入れ子になっているリソースを削除する前にリソースを削除することはできません。* "  
**原因**:イメージ バージョンを含むイメージ定義を削除しようとしました。 イメージ定義は削除する前に空にしておく必要があります。  
**回避策**:イメージ定義内のすべてのイメージ バージョンを削除してから、イメージ定義の削除に進みます。

**メッセージ**: "*パラメーター <property\> をバインドできません。値 <value\> を型 <propertyType\> に変換できません。識別子名 <value\> を有効な列挙子名と一致させることができません。次の列挙子名のいずれかを指定して、もう一度やり直してください: <choice1\>、<choice2\> など*"  
**原因**:プロパティに使用可能な値の制限一覧があり、<value\> はそのいずれでもありません。  
**回避策**:使用可能な <choice\> 値のいずれかを選択します。

**メッセージ**: "*パラメーター <property\> をバインドできません。値 <value\> を型 &quot;System.DateTime&quot; に変換できません*"  
**原因**:プロパティに指定された値が、日付として適切に書式設定されていません。  
**回避策**:yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz または [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) の有効な形式で日付を指定します。

**メッセージ**: "*パラメーター <property\> をバインドできません。値 <value\> を型 &quot;System.Int32&quot; に変換できません*"  
**原因**: <property\> には整数値のみを指定でき、<value\> は整数ではありません。  
**回避策**:整数値を選択します。

**メッセージ**: "*このリージョンでは、ZRS ストレージ アカウントの種類はサポートされていません。* "  
**原因**:標準のゾーン冗長ストレージ (ZRS) がまだサポートされていないリージョンで、それを選択しました。  
**回避策**:ストレージ アカウントの種類を **Premium\_LRS** または **Standard\_LRS** に変更します。 該当するドキュメントで、ZRS プレビューが有効になっている最新の[リージョンの一覧](../storage/common/storage-redundancy.md#zone-redundant-storage)を確認してください。

## <a name="creating-or-updating-image-versions"></a>イメージ バージョンの作成または更新 ##

**メッセージ**: "*指定された場所 <region\> は、リソースの種類 'Microsoft.Compute/galleries' では使用できません。リソースの種類に使用できるリージョンの一覧は ...* "  
**原因**:<region\> の名前が正しくないか、アクセス要求が必要です。  
**回避策**:リージョン名のスペルが正しいことを確認します。 このコマンドを実行して、アクセス権のあるリージョンを確認できます。 そのリージョンが一覧にない場合は、[アクセス要求](/troubleshoot/azure/general/region-access-request-process)を送信します。

**メッセージ**: "*ネストされたリソースに対して要求された操作を実行できません。親リソース <galleryName/imageDefinitionName\> が見つかりません。* "  
**原因**:現在のサブスクリプションとリソース グループに <galleryName/imageDefinitionName\> という名前のギャラリーがありません。  
**回避策**:ギャラリー、サブスクリプション、およびリソース グループの名前が正しいことを確認します。 それ以外の場合は、示されたリソース グループに、<galleryName\> という名前の新しいギャラリーと <imageDefinitionName\> という名前のイメージ定義、またはそのいずれかを作成します。

**メッセージ**: "*パラメーター <property\> をバインドできません。値 <value\> を型 &quot;System.DateTime&quot; に変換できません*"  
**原因**:プロパティに指定された値が、日付として適切に書式設定されていません。  
**回避策**:yyyy-MM-dd、yyyy-MM-dd'T'HH:mm:sszzz または [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) の有効な形式で日付を指定します。

**メッセージ**: "*パラメーター <property\> をバインドできません。値 <value\> を型 &quot;System.Int32&quot; に変換できません*"  
**原因**: <property\> には整数値のみを指定でき、<value\> は整数ではありません。  
**回避策**:整数値を選択します。

**メッセージ**: "*ギャラリー イメージ バージョンの公開プロファイル リージョン <publishingRegions\> には、イメージ バージョンの場所 <sourceRegion\> が含まれている必要があります*"  
**原因**:ソース イメージの場所 (<sourceRegion\>) は <publishingRegions\> 一覧に含まれている必要があります。  
**回避策**:<publishingRegions\> 一覧に <sourceRegion\> を含めます。

**メッセージ**: "*パラメーター <property\> の値 <value\> が範囲外です。値は <minValue\> と <maxValue\> の間 (両端を含む) である必要があります。* "  
**原因**: <value\> が <property\> に指定できる値の範囲外です。  
**回避策**:<minValue\> から <maxValue\> の範囲内 (両端を含む) の値を選択します。

**メッセージ**: "*ソース <resourceID\> が見つかりません。ソースが存在し、作成されているギャラリー イメージ バージョンと同じリージョンにあることを確認してください。* "  
**原因**:<resourceID\> にソースがないか、<resourceID\> にあるソースが、作成されているギャラリー イメージと同じリージョンにありません。  
**回避策**:<resourceID\> 値が正しいことと、ギャラリー イメージ バージョンのソース リージョンが <resourceID\> 値のリージョンと同じであることを確認します。

**メッセージ**: "*プロパティ 'galleryImageVersion.properties.storageProfile.<diskImage\>.source.id' の変更は許可されていません。* "  
**原因**:ギャラリー イメージ バージョンのソース ID は、作成後に変更することはできません。  
**回避策**:ソース ID が既存のソース ID と同じになるようにするか、イメージ バージョンのバージョン番号を変更するか、現在のイメージ バージョンを削除して再試行します。

**メッセージ**: "*入力データ ディスクで重複する LUN 番号が検出されました。LUN 番号は、データ ディスクごとに一意である必要があります。* "  
**原因**:ディスクとディスク スナップショット、またはそのいずれかの一覧を使用してイメージ バージョンを作成しているとき、2 つ以上のディスクまたはディスク スナップショットの LUN が同じです。  
**回避策**:重複する LUN を削除または変更します。

**メッセージ**: "*入力ディスクで重複するソース ID が検出されました。ソース ID は、ディスクごとに一意である必要があります。* "  
**原因**:ディスクとディスク スナップショット、またはそのいずれかの一覧を使用してイメージ バージョンを作成しているとき、2 つ以上のディスクまたはディスク スナップショットのリソース ID が同じです。  
**回避策**:重複するディスク ソース ID を削除または変更します。

**メッセージ**: "*パス 'properties.storageProfile.<diskImages\>.source.id' にあるプロパティ ID <resourceID\> が無効です。'/subscriptions/{subscriptionId}' または '/providers/{resourceProviderNamespace}/' で始まる完全修飾リソース ID を指定してください。* "  
**原因**:<resourceID\> 値の書式設定が正しくありません。  
**回避策**:リソース ID が正しいことを確認します。

**メッセージ**: "*ソース ID: <resourceID\> は、マネージド イメージ、仮想マシンまたは別のギャラリー イメージ バージョンである必要があります*"  
**原因**:<resourceID\> 値の書式設定が正しくありません。  
**回避策**:ソース イメージとして VM、マネージド イメージ、またはギャラリー イメージ バージョンを使用する場合は、VM、マネージド イメージ、またはギャラリー イメージ バージョンのリソース ID が正しいことを確認します。

**メッセージ**: "*ソース ID: <resourceID\> はマネージド ディスクまたはスナップショットである必要があります。* "  
**原因**:<resourceID\> 値の書式設定が正しくありません。  
**回避策**:イメージ バージョンのソースとしてディスクとディスク スナップショットまたはそのいずれかを使用する場合は、ディスクとディスク スナップショットまたはそのいずれかのリソース ID が正しいことを確認します。

**メッセージ**: "*親ギャラリー イメージの OS 状態 (<OsState\_1\>) が <OsState\_2\> ではないため、<resourceID\> からギャラリー イメージ バージョンを作成できません。* "  
**原因**:オペレーティング システムの状態 (一般化または特殊化) が、イメージ定義に指定されているオペレーティング システムの状態と一致しません。  
**回避策**:オペレーティング システムの状態が <OsState\_1\> の VM に基づいてソースを選択するか、<OsState\_2\> に基づいて VM の新しいイメージ定義を作成します。

**メッセージ**: "*ID '<resourceID\>' のリソースに、親ギャラリー イメージのハイパーバイザー世代 ['<V#\_2\>'] とは異なるハイパーバイザー世代 ['<V#\_1\>'] があります*"  
**原因**:イメージ バージョンのハイパーバイザー世代が、イメージ定義で指定されているハイパーバイザー世代と一致しません。 イメージ定義のオペレーティング システムが <V#\_1\> で、イメージ バージョンのオペレーティング システムが <V#\_2\> です。  
**回避策**:イメージ定義と同じハイパーバイザー世代のソースを選択するか、イメージ バージョンと同じハイパーバイザー世代の新しいイメージ定義を作成または選択します。

**メッセージ**: "*ID '<resourceID\>' のリソースに、親ギャラリー イメージの OS の種類の世代 ['<V#\_2\>'] とは異なる OS の種類 ['<V#\_1\>'] があります*"  
**原因**:イメージ バージョンのハイパーバイザー世代が、イメージ定義で指定されているハイパーバイザー世代と一致しません。 イメージ定義のオペレーティング システムが <OsType\_1\> で、イメージ バージョンのオペレーティング システムが <OsType\_2\> です。  
**回避策**:イメージ定義と同じオペレーティング システム (Linux または Windows) のソースを選択するか、イメージ バージョンと同じオペレーティング システム世代の新しいイメージ定義を作成または選択します。

**メッセージ**: "*ソース仮想マシン <resourceID\> には、エフェメラル OS ディスクを含めることはできません。* "  
**原因**:<resourceID\> のソースにエフェメラル OS ディスクが含まれています。 共有イメージ ギャラリーでは現在、エフェメラル OS ディスクはサポートされていません。  
**回避策**:エフェメラル OS ディスクを使用しない VM に基づいて、別のソースを選択してください。

**メッセージ**: "*ソース仮想マシン <resourceID\> には、UltraSSD という種類のアカウントに格納されているディスク ['<diskID\>'] を含めることはできません。* "  
**原因**:ディスク <diskID\> は Ultra SSD ディスクです。 共有イメージ ギャラリーでは現在、Ultra SSD ディスクはサポートされていません。  
**回避策**:Premium SSD、Standard SSD、および Standard HDD マネージド ディスクまたはそのいずれかのみを含むソースを使用します。

**メッセージ**: "*ソース仮想マシン<resourceID\> は Managed Disks から作成する必要があります。* "  
**原因**:<resourceID\> の仮想マシンでアンマネージド ディスクが使用されています。  
**回避策**:Premium SSD、Standard SSD、および Standard HDD マネージド ディスクまたはそのいずれかのみを含む VM に基づいてソースを使用します。

**メッセージ**: "*ソース '<resourceID\>' に対する要求が多すぎます。ソースに対する要求の数を減らすか、しばらく待ってから再試行してください。* "  
**原因**:このイメージ バージョンのソースは要求が多すぎるため、現在スロットルされています。  
**回避策**:後でイメージ バージョンの作成を試してみます。

**メッセージ**: "*ディスク暗号化セット '<diskEncryptionSetID\>' は、ギャラリー リソースと同じサブスクリプション '<subscriptionID\>' に存在する必要があります。* "  
**原因**:ディスク暗号化セットは、それが作成されたのと同じサブスクリプションおよびリージョンでのみ使用できます。  
**回避策**:イメージ バージョンと同じサブスクリプションおよびリージョンで暗号化セットを作成または使用します。

**メッセージ**: "*暗号化されたソース: '<resourceID\>' が、現在のギャラリー イメージ バージョンのサブスクリプション '<subscriptionID\_1\>' とは異なるサブスクリプション ID にあります。暗号化されていないソースで再試行するか、ソースのサブスクリプション '<subcriptionID\_2\>' を使用してギャラリー イメージ バージョンを作成してください。* "  
**原因**:共有イメージ ギャラリーでは現在、ソース イメージが暗号化されている場合、別のソース イメージからの別のサブスクリプションでのイメージ バージョンの作成はサポートされていません。  
**回避策**:暗号化されていないソースを使用するか、ソースと同じサブスクリプションでイメージ バージョンを作成します。

**メッセージ**: "*ディスク暗号化セット <diskEncryptionSetID\> が見つかりませんでした。* "  
**原因**:ディスクの暗号化が正しくない可能性があります。  
**回避策**:ディスク暗号化セットのリソース ID が正しいことを確認してください。

**メッセージ**: "*イメージ バージョン名が無効です。イメージ バージョン名は、Major(int).Minor(int).Patch(int) の形式にする必要があります。例: 1.0.0、2018.12.1 など*"  
**原因**:イメージ バージョンの有効な形式は、ピリオドで区切られた 3 つの整数です。 イメージ バージョン名が有効な形式を満たしていませんでした。  
**回避策**:Major(int).Minor(int).Patch(int) という形式に従ったイメージ バージョン名を使用します。 次に例を示します。1.0.0 または 2018.12.1。

**メッセージ**: "*パラメーター galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.dataDiskImages.diskEncryptionSetId の値が無効です*"  
**原因**:データ ディスク イメージで使用されているディスク暗号化セットのリソース ID に無効な形式が使用されています。  
**回避策**:ディスク暗号化セットのリソース ID が確実に、/subscriptions/<subscriptionID\>/resourceGroups/<resourceGroupName\>/providers/Microsoft.Compute/<diskEncryptionSetName\> という形式に従うようにします。

**メッセージ**: "*パラメーター galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.osDiskImage.diskEncryptionSetId の値が無効です。* "  
**原因**:OS ディスク イメージで使用されているディスク暗号化セットのリソース ID に無効な形式が使用されています。  
**回避策**:ディスク暗号化セットのリソース ID が確実に、/subscriptions/<subscriptionID\>/resourceGroups/<resourceGroupName\>/providers/Microsoft.Compute/<diskEncryptionSetName\> という形式に従うようにします。

**メッセージ**: "*ギャラリー イメージ バージョンの更新を要求する場合、リージョン [<region\>] のディスク暗号化セットを使用して新しいデータ ディスク イメージの暗号化 LUN [<number\>] を指定することはできません。このバージョンを更新するには、新しい LUN を削除します。データ ディスク イメージの暗号化設定を変更する必要がある場合は、正しい設定で新しいギャラリー イメージ バージョンを作成する必要があります。* "  
**原因**:既存のイメージ バージョンのデータ ディスクに暗号化を追加しました。 既存のイメージ バージョンに暗号化を追加することはできません。  
**回避策**:新しいギャラリー イメージ バージョンを作成するか、追加された暗号化設定を削除します。

**メッセージ**: "*ギャラリー成果物のバージョン ソースは、storageProfile で直接、あるいは個々の OS またはデータ ディスク内でのみ指定できます。1 つのソースの種類 (ユーザー イメージ、スナップショット、ディスク、仮想マシン) のみを指定できます。* "  
**原因**:ソース ID が欠落しています。  
**回避策**:必ず、ソースのソース ID が存在するようにしてください。

**メッセージ**: "*ソースが見つかりませんでした: <resourceID\>。ソースが存在することを確認してください。* "  
**原因**:ソースのリソース ID が正しくない可能性があります。  
**回避策**:ソースのリソース ID が正しいことを確認してください。

**メッセージ**: "*ディスク暗号化セット '<diskEncryptionSetID\>' がリージョン '<Region\_2\>' の対応するディスクに使用されているため、ターゲット リージョン '<Region\_1\>' のディスク 'galleryArtifactVersion.properties.publishingProfile.targetRegions.encryption.osDiskImage.diskEncryptionSetId' にはディスク暗号化セットが必要です*"  
**原因**:暗号化は <Region\_2\> の OS ディスクで使用されていましたが、<Region\_1\> では使用されていません。  
**回避策**:OS ディスクで暗号化を使用する場合は、すべてのリージョンで暗号化を使用します。

**メッセージ**: "*ディスク暗号化セット '<diskEncryptionSetID\>' がリージョン '<Region\_2\>' の対応するディスクに使用されているため、ターゲット リージョン '<Region\_1\>' のディスク 'LUN <number\>' にはディスク暗号化セットが必要です"*  
**原因**:暗号化は <Region\_2\> の LUN <number\> のデータ ディスクで使用されていましたが、<Region\_1\> では使用されていません。  
**回避策**:データ ディスクで暗号化を使用する場合は、すべてのリージョンで暗号化を使用します。

**メッセージ**: "*無効な LUN [<number\>] が encryption.dataDiskImages に指定されました。LUN は次のいずれかの値である必要があります: ['0、9']。* "  
**原因**:暗号化に指定された LUN は、VM にアタッチされているディスクのどの LUN とも一致しません。  
**回避策**:暗号化の LUN を、VM に存在するデータ ディスクの LUN に変更します。

**メッセージ**: "*ターゲット リージョン '<region\>' の encryption.dataDiskImages に重複する LUN '<number\>' が指定されました。* "  
**原因**:<region\> で使用されている暗号化設定で、LUN が少なくとも 2 回指定されました。  
**回避策**:<region\> の LUN を変更し、すべての LUN が <region\> で一意であるようにします。

**メッセージ**: "*OSDiskImage と DataDiskImage で同じ BLOB <sourceID\> を指定することはできません*"  
**原因**:OS ディスクと少なくとも 1 つのデータ ディスクのソースが一意ではありません。  
**回避策**:OS ディスクおよびデータ ディスク、またはそのいずれかのソースを変更し、OS ディスクと各データ ディスクが一意になるようにします。

**メッセージ**: "*ターゲットの公開リージョンで重複するリージョンは許可されません。* "  
**原因**:公開リージョンの一覧に含まれるリージョンが複数回表示されています。  
**回避策**:重複するリージョンを削除します。

**メッセージ**: "*新しいデータ ディスクを追加したり、既存のイメージ内のデータ ディスクの LUN を変更したりすることはできません。* "  
**原因**:イメージ バージョンの更新呼び出しに、新しいデータ ディスクが含まれているか、ディスクの新しい LUN があります。  
**回避策**:既存のイメージ バージョンの LUN とデータ ディスクを使用します。

**メッセージ**: "*ディスク暗号化セット <diskEncryptionSetID\> は、ギャラリー リソースと同じサブスクリプション <subscriptionID\> に存在する必要があります。* "  
**原因**:共有イメージ ギャラリーでは現在、別のサブスクリプションでのディスク暗号化セットの使用はサポートされていません。  
**回避策**:同じサブスクリプションにイメージ バージョンとディスク暗号化セットを作成します。

**メッセージ**: *'GalleryImageVersion ソース リソースのサイズ 2048 はサポートされている最大サイズ 1024 を超えている' ため、このリージョンでレプリケーションが失敗しました。*  
**原因**: ソース内のデータ ディスクが 1 TB を超えています。  
**回避策**: データ ディスクのサイズを 1 TB 未満に変更します。

## <a name="creating-or-updating-a-vm-or-scale-sets-from-an-image-version"></a>イメージ バージョンからの VM またはスケール セットの作成または更新 ##

**メッセージ**: *"<imageDefinitionResourceID\>" の最新のイメージ バージョンがありません*"  
**原因**: 最新に含まれているイメージ バージョンが、仮想マシンのデプロイに使用したイメージ定義にどれも含まれていません。  
**回避策**: [最新から除外] が False に設定されているイメージ バージョンが少なくとも 1 つ存在するようにしてください。 

**メッセージ**: *"クライアントには、スコープ <resourceID\> に対するアクション 'Microsoft.Compute/galleries/images/versions/read' を実行する権限がありますが、現在のテナント <tenantId1\> には、リンクされたサブスクリプション <subscriptionId2\> にアクセスする権限がありません。* "  
**原因**:仮想マシンまたはスケール セットは、別のテナントの SIG イメージを使用して作成されました。 仮想マシンまたはスケール セットを変更しようとしましたが、イメージを所有しているサブスクリプションへのアクセス権がありません。  
**回避策**:イメージ バージョンのサブスクリプションの所有者に連絡して、イメージ バージョンに対する読み取りアクセス権を付与するようにしてください。

**メッセージ**: "*ギャラリー イメージ <resourceID\> は <region\> リージョンで使用できません。イメージ所有者に連絡して、このリージョンにレプリケートするか、要求されたリージョンを変更するようにしてください。* "  
**原因**:ギャラリー イメージの公開されたリージョンの一覧に含まれていないリージョンに VM が作成されています。  
**回避策**:イメージをリージョンにレプリケートするか、ギャラリー イメージの公開リージョンのいずれかのリージョンに VM を作成します。

**メッセージ**: "*パラメーター 'osProfile' は許可されていません。* "  
**原因**:特殊化されたイメージ バージョンから作成された VM に対して、管理者のユーザー名、パスワード、または SSH キーが指定されました。  
**回避策**:そのイメージから VM を作成する場合は、管理者のユーザー名、パスワード、SSH キーを含めないでください。 それ以外の場合は、一般化されたイメージ バージョンを使用し、管理者のユーザー名、パスワード、または SSH キーを指定します。

**メッセージ**: "*必須パラメーター 'osProfile' がありません (null 値)。* "  
**原因**:VM が一般化されたイメージから作成されており、管理者のユーザー名、パスワード、または SSH キーがありません。 一般化されたイメージには管理者のユーザー名、パスワード、SSH キーが保持されないため、これらのフィールドは VM またはスケール セットの作成時に指定する必要があります。  
**回避策**:管理者のユーザー名、パスワード、または SSH キーを指定するか、特殊化されたイメージ バージョンを使用します。

**メッセージ**: "*親ギャラリー イメージの OS 状態 ('特殊化') が '一般化' ではないため、<resourceID\> からギャラリー イメージ バージョンを作成できません。* "  
**原因**:イメージ バージョンは一般化されたソースから作成されていますが、その親定義は特殊化されています。  
**回避策**:特殊化されたソースを使用してイメージ バージョンを作成するか、一般化された親定義を使用します。

**メッセージ**: "*仮想マシン スケール セット <vmssName\> を更新できません。VM スケール セットの現在の OS 状態は一般化であり、これは更新後のギャラリー イメージの OS 状態の特殊化と異なるためです。* "  
**原因**:スケール セットの現在のソース イメージは一般化されたソース イメージですが、特殊化されたソース イメージで更新されています。 スケール セットの現在のソース イメージと新しいソース イメージは、同じ状態である必要があります。  
**回避策**:スケール セットを更新するには、一般化されたイメージ バージョンを使用します。

**メッセージ**: "*共有イメージ ギャラリー <versionId\> のディスク暗号化セット <diskEncryptionSetId\> はサブスクリプション <subscriptionId1\> に属しているため、サブスクリプション <subscriptionId2\> のリソース '' で使用することはできません*"  
**原因**:イメージ バージョンの暗号化に使用されるディスク暗号化セットが、イメージ バージョンをホストするサブスクリプションとは別のサブスクリプションに存在します。  
**回避策**:イメージ バージョンとディスク暗号化セットに同じサブスクリプションを使用します。

**メッセージ**: "*VM または仮想マシン スケール セットの作成に長い時間がかかる。* "  
**回避策**:VM または仮想マシン スケール セットの作成を試行している元のイメージ バージョンの **OSType** が、イメージ バージョンの作成に使用したソースの **OSType** と同じであることを確認します。 

## <a name="creating-a-disk-from-an-image-version"></a>イメージ バージョンからのディスクの作成 ##

**メッセージ**: "*パラメーター imageReference の値が無効です。* "  
**原因**:SIG イメージ バージョンからディスクにエクスポートしようとしましたが、そのイメージには存在しない LUN の位置が使用されています。    
**回避策**:イメージ バージョンを確認して、使用されている LUN の位置を確認します。

## <a name="sharing-resources"></a>リソースを共有する

サブスクリプション全体でのイメージ ギャラリー、イメージ定義、およびイメージ バージョンのリソースの共有が、[Azure ロールベースのアクセス制御 (Azure RBAC)](../role-based-access-control/rbac-and-directory-admin-roles.md) を使用して有効にされています。 

## <a name="replication-speed"></a>レプリケーション速度

**-expand ReplicationStatus** フラグを使用して、指定されたすべてのターゲット リージョンへのレプリケーションが完了したかどうかを確認します。 完了していない場合は、ジョブが完了するまで最大 6 時間待機します。 レプリケートがエラーになった場合、コマンドをもう一度トリガーし、イメージ バージョンを作成してレプリケートします。 イメージ バージョンのレプリケート先となるターゲット リージョンが多数ある場合、段階的にレプリケーションを行うことを検討します。

## <a name="azure-limits-and-quotas"></a>Azure の制限とクォータ 

[Azure の制限とクォータ](../azure-resource-manager/management/azure-subscription-service-limits.md)は、すべての共有イメージ ギャラリー、イメージ定義、およびイメージ バージョンのリソースに適用されます。 お使いのサブスクリプションの制限内であることを確認してください。 


## <a name="next-steps"></a>次のステップ

詳細については、[共有イメージ ギャラリー](./shared-image-galleries.md)に関するページを参照してください。