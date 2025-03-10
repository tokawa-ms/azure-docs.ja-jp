---
title: 合成音声テクノロジの責任あるデプロイのためのガイドライン
titleSuffix: Azure Cognitive Services
description: 合成音声テクノロジを使用するための Microsoft の一般的な設計ガイドライン。 これらは、合成音声の責任ある開発を導くために、Microsoft がボイス タレント、消費者、および発話障害のある人々と共に行った研究で開発されました。
services: cognitive-services
author: benoah
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: benoah
ms.openlocfilehash: 3a0b645acd7c21ff0416c748cdd2caf7041be508
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101730815"
---
# <a name="guidelines-for-responsible-deployment-of-synthetic-voice-technology"></a>合成音声テクノロジの責任あるデプロイのためのガイドライン

この記事では、合成音声テクノロジを使用するための Microsoft の一般的な設計ガイドラインについて説明します。 これらのガイドラインは、合成音声の責任ある開発を導くために、Microsoft がボイス タレント、消費者、および発話障害のある人々と共に行った研究で開発されました。

合成音声テクノロジのデプロイでは、ほとんどのシナリオで次のガイドラインが適用されます。

### <a name="disclose-when-the-voice-is-synthetic"></a>音声が合成音声の場合に開示する
音声がコンピューターによって生成されたものであることを開示することにより、偽装による有害な結果を招くリスクを最小限に抑えられるだけでなく、音声を発信する組織への信頼も高まります。 [開示する方法](concepts-disclosure-guidelines.md)の詳細をご覧ください。

Microsoft はお客様に、カスタム ニューラル音声が合成であることをユーザーに明示することをお願いします。 
* 特に、よく知られた人物の声を使用する場合は、対象ユーザーに十分な開示を提供するようにしてください。人は、意識的であれ無意識的であれ、誰から情報が提供されるのかに基づいて情報についての判断を下すものです。  たとえば、最初に広まるときは口頭で共有される可能性があります。 詳細については、[開示パターン](concepts-disclosure-patterns.md)に関するページを参照してください。   
* 未成年者や子供向けに設計されたユース ケースについて、親や他の関係者に適切に開示することを検討します。ユース ケースで未成年者または子供が意図されている場合は、親または法的保護者が合成メディアの使用に関する開示を理解し、未成年者または子供がエクスペリエンスを使用するかどうかについて、適切な判断を下すことができます。 

### <a name="select-appropriate-voice-types-for-your-scenario"></a>シナリオに適した音声の種類を選択する
使用状況と、合成音声の使用に関連する潜在的な有害性について慎重に検討してください。 たとえば、忠実度の高い合成音声は、個人的なメッセージ、金融取引、人間の適応性や共感を必要とする複雑な状況など、リスクの高いシナリオには適していない場合があります。 

また、音声の種類に対するユーザーの期待も異なる場合があります。 たとえば、合成音声で読まれるデリケートな内容のニュースを聞くときに、共感的で人間に近い語調を好む人がいる一方で、偏りのない音声を好む人もいます。 ユーザーの好みをよりよく理解するために、アプリケーションのテストを検討してください。

### <a name="be-transparent-about-capabilities-and-limitations"></a>機能と制限について透明性を保つ
ユーザーは忠実度の高い合成音声エージェントと対話するときに、より高い期待を持つ傾向があります。 システムの機能がこのような期待に応えられない場合、信頼が損なわれ、不快な経験となったり、有害な経験をもたらす可能性があります。

### <a name="provide-optional-human-support"></a>人的サポートをオプションで提供する
あいまいなトランザクションのシナリオ (たとえばコール サポート センター) では、ユーザーはコンピューター エージェントが自分の要求に適切に応答することを常に信頼するわけではありません。 このような状況では、システムの音声や機能の現実的な品質に関係なく、人的サポートが必要になる場合があります。

## <a name="considerations-for-voice-talent"></a>ボイス タレントに関する考慮事項
合成音声を作成するために声優などのボイス タレントを起用する場合、次のガイドラインが適用されます。

### <a name="obtain-meaningful-consent-from-voice-talent"></a>ボイス タレントから有意な同意を得る
ボイス タレントは、自分の音声モデル (その使用方法と場所) を管理し、それが使用されたら報酬を受け取る必要があります。 Microsoft は、Custom Voice のお客様に、合成音声の作成についてボイス タレントから明示的に書面による許可を取得し、ボイス タレントとの同意では期間、使用、コンテンツの制限について熟考することを要求しています。  よく知られた人物の合成音声を作成する場合は、音声の基になっている人物がコンテンツを編集または承認する手段を提供する必要があります。

一部のボイス タレントは、このテクノロジが悪用される可能性があることを認識しておらず、システム所有者からこのテクノロジの機能について教育を受ける必要があります。 Microsoft は、お客様に対し、合成音声の開発方法およびテキスト読み上げサービスでの動作方法について説明されている Microsoft の[ボイス タレントに対する開示](/legal/cognitive-services/speech-service/disclosure-voice-talent)を、ボイス タレントと直接、またはボイス タレントの正式な代理人を通じて、共有することを要求します。

## <a name="considerations-for-those-with-speech-disorders"></a>発話障碍のある個人についての考慮事項
発話障害のある個人と連携して合成音声テクノロジを作成またはデプロイするには、次のガイドラインが適用されます。

### <a name="provide-guidelines-to-establish-contracts"></a>契約を確立するためのガイドラインを提供する
発話の支援として合成音声を使用する個人との契約を確立するためのガイドラインを提供します。 この契約では、音声を所有する当事者、使用期間、所有権の譲渡条件、音声フォントを削除する手順、および不正アクセスを防ぐ方法を指定することを検討する必要があります。 さらに、権限が与えられている場合は、死亡後に家族のメンバーに音声フォントの所有権を譲渡することができます。

### <a name="account-for-inconsistencies-in-speech-patterns"></a>音声パターンの一貫性の欠如について考慮する
発話障害のある個人が自分の音声フォントを記録する場合、音声パターンの一貫性の欠如 (特定の単語が不明瞭な発音になる、または発音できないこと) が原因で、録音プロセスが複雑になる可能性があります。 このような場合は、合成音声テクノロジと録音セッションで、それらに対応する必要があります (つまり、休憩を入れて録音セッションの回数を増やします)。

### <a name="allow-modification-over-time"></a>時間の経過に伴う変化を考慮に入れる
発話障害のある個人は、加齢を反映するために自分の合成音声を更新したいと考えることがあります (たとえば、子供が思春期を迎えた場合など)。 また、様式的な好みが時間の経過と共に変化することもあり、ピッチやアクセントなどの音声特性に変更を加えたい場合もあります。


## <a name="see-also"></a>関連項目

* [ボイス タレント向けの開示](/legal/cognitive-services/speech-service/disclosure-voice-talent?context=%2fazure%2fcognitive-services%2fspeech-service%2fcontext%2fcontext)
* [開示する方法](concepts-disclosure-guidelines.md)
* [設計パターンを開示する](concepts-disclosure-patterns.md)