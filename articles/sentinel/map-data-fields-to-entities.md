---
title: データ フィールドを Azure Sentinel エンティティにマップする |Microsoft Docs
description: インシデント情報の質を向上させるために、テーブルのデータ フィールドを分析ルールの Azure Sentinel エンティティにマップする
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/10/2021
ms.author: yelevin
ms.openlocfilehash: cb91d269f6b166510db54637d17d776e71137408
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2021
ms.locfileid: "102456180"
---
# <a name="map-data-fields-to-entities-in-azure-sentinel"></a>データ フィールドを Azure Sentinel のエンティティにマップする 

> [!IMPORTANT]
>
> - 新しいバージョンのエンティティ マッピング機能は "**プレビュー**" 段階にあります。 ベータ版、プレビュー版、または一般提供としてまだリリースされていない Azure の機能に適用されるその他の法律条項については、「[Microsoft Azure プレビューの追加使用条件](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)」を参照してください。

> [!IMPORTANT]
>
> - 新しいバージョンと古いバージョンの間のエンティティ マッピングの下位互換性と相違点に関する重要な情報については、このドキュメントの最後にある「[新しいバージョンに関する注意事項](#notes-on-the-new-version)」を参照してください。

## <a name="introduction"></a>はじめに

エンティティ マッピングは、[スケジュールされたクエリ分析ルール](tutorial-detect-threats-custom.md)の構成に不可欠な要素です。 これにより、ルールの出力 (アラートとインシデント) が、その後の調査プロセスと修正アクションの構成要素として機能する重要な情報で強化されます。

以下で詳しく説明する手順は、分析ルールの作成ウィザードの一部です。 ここでは、既存の分析ルールでエンティティ マッピングを追加または変更するシナリオに対処するために、個別に扱います。

## <a name="how-to-map-entities"></a>エンティティをマップする方法

1. Azure Sentinel のナビゲーション メニューから **[分析]** を選択します。

1. スケジュールされたクエリ ルールを選択し、 **[編集]** をクリックします。 または、画面の上部にある **[スケジュールされたクエリ ルールの作成]** をクリックして新しいルールを作成します。

1. **[ルール ロジックの設定]** タブをクリックします。

    :::image type="content" source="media/map-data-fields-to-entities/map-entities.png" alt-text="フィールドをエンティティにマップする":::

1. **[アラートの拡張]** セクションの **[エンティティ マッピング]** で、 **[エンティティの種類]** ドロップダウン リストからエンティティの種類を選択します。

1. エンティティの "**識別子**" を選択します。 識別子はエンティティの属性であり、それを十分に識別できるものです。 **[識別子]** ドロップダウン リストからいずれかを選択してから、 **[値]** ドロップダウン リストから識別子に対応するデータ フィールドを選択します。 いくつかの例外がありますが、 **[値]** の一覧は、ルール クエリの対象として定義されているテーブルのデータ フィールドによって設定されます。

    特定のエンティティに対して "**最大 3 つの識別子**" を定義できます。 一部の識別子は必須であり、それ以外は省略可能です。 少なくとも 1 つの必須の識別子を選択する必要があります。 そうしないと、警告メッセージによって必須の識別子が示されます。 最良の結果を得るためには (一意の識別を最大限にするためには)、可能な限り "**強力**" な識別子を使用してください。複数の強力な識別子を使用すると、データ ソース間の相関関係が高まりますます。 使用できる[エンティティと識別子](entities-reference.md)の完全な一覧を参照してください。

1. **[新しいエンティティの追加]** をクリックして、さらに多くのエンティティをマップします。 1 つの分析ルールで "**最大 5 つのエンティティ**" をマップできます。 複数の同じ種類のものをマップすることもできます。 たとえば、1 つは "*発信元 IP アドレス*" フィールドから、もう 1 つは "*宛先 IP アドレス*" フィールドからの、2 つの "**IP**" エンティティをマップすることができます。 このようにして、両方を追跡することができます。

    気が変わった場合やミスをしてしまった場合は、エンティティのドロップダウン リストの横にあるごみ箱アイコンをクリックして、エンティティ マッピングを削除できます。

1. エンティティのマッピングが完了したら、 **[確認と作成]** タブをクリックします。ルールの検証が成功したら、 **[保存]** をクリックします。

## <a name="notes-on-the-new-version"></a>新しいバージョンに関する注意事項

- この分析ルールのエンティティ マッピングを、古いバージョンを使用して定義したことがある場合は、それらのマッピングがクエリ コードに表示されます。 新しいバージョンで定義されたエンティティ マッピングは、"**クエリ コードに表示されません**"。 分析ルールでは一度に 1 つのバージョンのエンティティ マッピングのみがサポートされ、新しいバージョンが優先されます。 そのため、ここで定義する任意の 1 つのマッピングによって、クエリの実行時にクエリ コードで定義された "**すべて**" のマッピングが "**無視**" されます。 

- それでも "**古いバージョン**" のエンティティ マッピングを使用する必要がある場合は (新しいバージョンがまだプレビュー段階にある限り)、URL で機能フラグを使用して引き続きアクセスすることができます。 `https://portal.azure.com/` と `#blade` の間にカーソルを置き、`?feature.EntityMapping=false` というテキストを挿入します。

  - 古いバージョンの制限は引き続き適用されます。 マップできるのは、ユーザー、ホスト、IP アドレス、URL、およびファイル ハッシュのエンティティだけで、それぞれ 1 つだけです。

  - 古いバージョンに戻す "**前**" に、新しいバージョンを使用して作成したエンティティ マッピングをすべて "**削除**" する必要があります。そうしないと、古いバージョンを使用するエンティティ マッピングは "**機能しません**"。

- 新しいバージョンのエンティティ マッピングが一般公開されると、古いバージョンを使用できなくなります。 古いエンティティ マッピングを新しいバージョンに移行することを強くお勧めします。


## <a name="next-steps"></a>次のステップ

このドキュメントでは、Azure Sentinel 分析ルールでデータ フィールドをエンティティにマップする方法について説明しました。 Azure Sentinel の詳細については、次の記事をご覧ください。
- [スケジュールされたクエリ分析ルール](tutorial-detect-threats-custom.md)の完全な画像を取得します。
- [Azure Sentinel のエンティティ](entities-in-azure-sentinel.md)について詳しく確認します。
