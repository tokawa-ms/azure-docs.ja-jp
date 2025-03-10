---
title: Azure Application Insights の使用の影響 | Microsoft Docs
description: さまざまなプロパティがアプリの各部のコンバージョン率に潜在的に及ぼす影響について分析します。
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/08/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 51f938743d8da3c9a1dea546320662701e2b88c8
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/17/2021
ms.locfileid: "100583512"
---
# <a name="impact-analysis-with-application-insights"></a>Application Insights による影響分析

影響では、読み込み時間とその他のプロパティがアプリのさまざまな部分のコンバージョン率に及ぼす影響について分析します。 より正確に言えば、**ページ ビュー**、**カスタム イベント**、または **要求** の **ディメンション** が、異なる **ページ ビュー** または **カスタム イベント** の使用にどの程度影響するのかについて、明らかにします。 

![影響ツール](./media/usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>影響で行われることがまだわからない場合

影響については、サイトの一部における遅さがユーザーの定着にどの程度影響しているのかについて、チームのメンバーとの議論を収める究極のツールとして考えるのが、1 つの方法です。 ユーザーが一定の遅さを許容する場合がありますが、影響によって、最適化とパフォーマンスのバランスを最善にしてユーザーのコンバージョン率を最大化する方法についての洞察が得られます。

しかし、パフォーマンスの分析は影響の機能の一部にすぎません。 影響ではカスタム イベントとディメンションがサポートされているため、ユーザーのブラウザーの選択と異なるコンバージョン率がどのように相関しているかなどの質問に、わずか数クリックで答えることができます。

![ブラウザーごとのコンバージョンのスクリーンショット](./media/usage-impact/0004-browsers.png)

> [!NOTE]
> 影響ツールを使うには、Application Insights のリソースにページ ビューまたはカスタム イベントが含まれる必要があります。 [アプリをセットアップし、Application Insights JavaScript SDK を使用してページ ビューを自動的に収集する方法について説明します](./javascript.md)。 また、相関関係を分析しているので、サンプルのサイズが問題になることにも注意してください。
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>ページの読み込み時間がページでのコンバージョン数に影響しているか

影響ツールを使って質問への回答を始めるには、初期ページ ビュー、カスタム イベント、または要求を選択します。

![最初のページ ビュー、カスタム イベント、または要求を選択する場所を示すスクリーンショット。](./media/usage-impact/0002-dropdown.png)

1. **[For the page view]\(ページ ビューについて\)** ドロップダウンからページ ビューを選択します。
2. **[次を分析]** ドロップダウンを既定の選択である **[期間]** のままにします (このコンテキストでは、 **[期間]** は **ページの読み込み時間** のエイリアスです)。
3. **[次の使用への影響]** ドロップダウンで、カスタム イベントを選択します。 このイベントは、手順 1. で選択したページ ビューの UI 要素に対応する必要があります。

![結果のスクリーンショット](./media/usage-impact/0003-results.png)

この例では、**製品ページ** の読み込み時間の増加に伴って、**クリックされた購入製品** へのコンバージョン率が低下しています。 上記の分布に基づくと、潜在的な 55 % のコンバージョン率を実現するために、3.5 秒の最適なページ読み込み時間をターゲットにできます。 現在、さらにパフォーマンスを向上させて読み込み時間を 3.5 秒未満に減らしたとしても、それに伴ってコンバージョンのメリットが向上することはありません。

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>カスタムの方法でページ ビューまたは読み込み時間を追跡する場合

影響では、標準とカスタムの両方のプロパティと測定値がサポートされています。 お好きな方をお使いください。 期間の代わりに 1 つ目と 2 つ目のイベントでフィルターを使用すると、より具体的な情報が得られます。

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>ユーザーの国またはリージョンの違いがコンバージョン率の違いに影響するか

1. **[For the page view]\(ページ ビューについて\)** ドロップダウンからページ ビューを選択します。
2. **[次を分析]** ドロップダウンで [国またはリージョン] を選択します
3. **[次の使用への影響]** ドロップダウンでは、手順 1. で選択したページ ビューの UI 要素に対応するカスタム イベントを選択します。

この場合の結果は、最初の例のような連続する x 軸のモデルに適合しません。 その代わり、セグメント化されたファネルに似た視覚化が表示されます。 **[Usage]\(使用\)** で並べ替えて、国や地域に基づいたカスタム イベントへのコンバージョンのバリエーションを表示します。


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>影響ツールによってこれらのコンバージョン率が計算される方法

内部的には、影響ツールは[ピアソン相関関係の係数](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)に依存しています。 結果は -1 と 1 の間で計算されます。ここでは、-1 が負の線形相関を表し、1 が正の線形相関を表します。

影響分析のしくみを単純に分解すると、次のようになります。

_A_ = 最初のドロップダウンで選択するメインのページ ビュー/カスタム イベント/要求とします。 (**ページ ビューについて**)。

_B_ = 選択する 2 つ目のページ ビュー/カスタム イベント (**次の使用への影響**) とします。

選択された時間の範囲で、ユーザーからの全セッションのサンプルが影響によって確認されます。 各セッションについて、_A_ のそれぞれの発生が検索されます。

セッションは、2 つの条件のいずれかに基づいて、2 つの異なる種類の "_サブセッション_" に分けられます。

- コンバージョンが行われたサブセッションは、_B_ イベントで終わったセッションで構成され、_B_ の前に発生したすべての _A_ イベントを含みます。
- コンバージョンが行われなかったサブセッションは、_B_ で終わらず、すべての _A_ が発生した場合に発生します。

最終的に影響がどのように計算されるかは、メトリックにより分析しているのか、それともディメンションにより分析しているのかに基づいて異なります。 メトリックでは、サブセッション内のすべての _A_ は平均化されます。 一方ディメンションでは、各 _A_ の値は、_B_ に割り当てられた値への _1/N_ に貢献します。ここで、_N_ はサブセッション内の _A_ の数です。

## <a name="next-steps"></a>次のステップ

- 使用状況を把握できるようにするには、[カスタム イベント](./api-custom-events-metrics.md#trackevent)または[ページ ビュー](./api-custom-events-metrics.md#page-views)の送信を開始します。
- カスタム イベントまたはページ ビューを既に送信した場合は、使用状況ツールを見て、自分のサービスがユーザーにどのように使用されているかを把握します。
    - [ファネル](usage-funnels.md)
    - [保持](usage-retention.md)
    - [ユーザー フロー](usage-flows.md)
    - [ブック](../visualize/workbooks-overview.md)
    - [ユーザー コンテキストの追加](usage-send-user-context.md)

